---
title: AWS serverless로 nlp 모델 배포하기
date: 2021-12-12 21:57:00 +0900
categories: [nlp]
tags: [nlp, aws, ml serving]
use_math: true
---
AWS 배포시에 다음과 같은 장단이 있다
속도(구현 속도) : AWS(lambda) - 싸다, 구현하기 쉽다(파이썬 스크립트를 짜고)

- 문제 : Deeplearning 모델을 서빙하기에 적합하지 않다(cpu)
- 30초안에 리턴이 반환되어야 한다.

제품화 할때는 어떤 조합?
- tensorflow.js
- **fastapi + pytorch(tensor) - 구현예정**
- django(+orm) + pytorch(tensor)
- flask + tensor(pytorch)
- **flask + ml**
- docker

## 1. serverless 설치

- AWS에서 템플릿이용해서 serverless instance를 띄우는 것을 도와주는 라이브러리
- AWS → 너무 명령어가 복잡(단계가 좀 많다) ⇒ serverless를 하기위해서 좀 편하게 템플릿을 잡아주는 것(라이브러리)

```jsx
# mac os 기준
npm install -g serverless
```

- 설치하게되면 AWS credentials 를 가져와서 기본 세팅함
- 없다면(AWS CLI가 세팅 안된경우) : [https://www.serverless.com/framework/docs/providers/aws/guide/credentials/](https://www.serverless.com/framework/docs/providers/aws/guide/credentials/)

## 2. Local dir에 템플릿 생성

```bash
serverless create --template aws-python3 ## --path serverless-bert (경로를 제공)

# 현재 workdir에 생성
# serverless create --template aws-python3

# 별도 경로에 생성
# serverless create --template aws-python3 --path [new_workdir_path]
```

<a href="https://imgbb.com/"><img src="https://i.ibb.co/LScvZxD/Untitled.png" alt="Untitled" border="0"></a>

- 실행시
- `handler.py`
    - lambda 실행을 위한 boilerplate code가 들어있음
- serverless.yml
- 생성됨

## 3. Task에 맞게 handler function 정의

- code
    
    ```bash
    import json
    import torch
    from transformers import GPT2LMHeadModel, PreTrainedTokenizerFast
    
    def encode(tokenizer, context):
        """encodes the question and context with a given tokenizer"""
        encoded = tokenizer.encode(context)
        return encoded
    
    def decode(tokenizer, tokens):
        """decodes the tokens to the answer with a given tokenizer"""
        return tokenizer.decode(tokens[0,:].tolist())
    
    def serverless_pipeline(model_path='./model'):
        MODEL_NAME = 'skt/kogpt2-base-v2'
        """Initializes the model and tokenzier and returns a predict function that ca be used as pipeline"""
        model = GPT2LMHeadModel.from_pretrained(MODEL_NAME)
        tokenizer = PreTrainedTokenizerFast.from_pretrained(MODEL_NAME,
            bos_token='</s>', eos_token='</s>', unk_token='<unk>',
            pad_token='<pad>', mask_token='<mask>')
        def gen_result(context_dict):
            def predict(context, max_len, pen=2.0):
                """predicts the answer on an given question and context. Uses encode and decode method from above"""
                input_ids = tokenizer.encode(tokenizer, context)
                gen_ids = model.generate(torch.tensor([input_ids]),
                                    max_length=max_len,
                                    repetition_penalty=pen,
                                    pad_token_id=tokenizer.pad_token_id,
                                    eos_token_id=tokenizer.eos_token_id,
                                    bos_token_id=tokenizer.bos_token_id,
                                    use_cache=True)
                answer = tokenizer.decode(gen_ids)
                return answer
            predict_set = {
                'gen_text': [predict(context_dict['main_title'], max_len=90, pen=i)]
            }
            return predict_set
        return gen_result
    
    gpt_pipeline = serverless_pipeline()
    
    def handler(event):
        try:
            # loads the incoming event into a dictonary
            body = json.loads(event['body'])
            # uses the pipeline to predict the answer
            print(body)
            answer = gpt_pipeline(context_dict=body['context'])
            return {
                "statusCode": 200,
                "headers": {
                    'Content-Type': 'application/json',
                    'Access-Control-Allow-Origin': '*',
                    "Access-Control-Allow-Credentials": True
    
                },
                "body": json.dumps({'answer': answer})
            }
        except Exception as e:
            print(repr(e))
            return {
                "statusCode": 500,
                "headers": {
                    'Content-Type': 'application/json',
                    'Access-Control-Allow-Origin': '*',
                    "Access-Control-Allow-Credentials": True
                },
                "body": json.dumps({"error": repr(e)})
            }
    
        # Use this code if you don't use the http event with the LAMBDA-PROXY
        # integration
        """
        return {
            "message": "Go Serverless v1.0! Your function executed successfully!",
            "event": event
        }
        """
    ```
    

## 4. dockerbuild 및 테스트

```bash
# docker build
docker build -t [docker-tag] .

# docker run
docker run -p 8080:8080 [docker-tag]

# check docker container using ssh
docker exec -it [container_name] /bin/bash
```

- [https://github.com/aws/aws-lambda-runtime-interface-emulator/](https://github.com/aws/aws-lambda-runtime-interface-emulator/)
- aws 에서 에뮬레이터를 제공함, 그러나 생각보다 안정적이지는 않아서 잘 쓰지는 않았음

- lambda 형식에 맞는 모델 코드를 작성
- 이후 dockerfile로 묶기
    - 인풋 → 추론 파이썬코드
    - 모델파일 (gpt2)
- aws → dockerfile 올리기 (ECR)

## 5. ECR 생성

```bash
# 리포지토리 생성
aws ecr create-repository --repository-name [docker-tag] > /dev/null

# aws region information
aws_region=[AWS_REGION]
aws_account_id=[AWS_ACCOUNT_ID]

# aws ecr 로그인
aws ecr get-login-password \
    --region $aws_region \
| docker login \
    --username AWS \
    --password-stdin $aws_account_id.dkr.ecr.$aws_region.amazonaws.com

# Login Succeeded

# docker tag
docker tag [docker-tag] $aws_account_id.dkr.ecr.$aws_region.amazonaws.com/[docker-tag]
# docker push
docker push $aws_account_id.dkr.ecr.$aws_region.amazonaws.com/[docker-tag]
# or
docker push $aws_account_id.dkr.ecr.$aws_region.amazonaws.com/[docker-tag]:version
```

## 6. serverless deploy

- aws ecr - dockerfile
    - ecr - lambda - api gateway ← API
- serverless file이 있는 dir에서

```bash
# work dir
serverless deploy
```

- 서버리스 deploy를 사용하지 않는다면, 참고 [https://blog.algopie.com/aws/aws-lambda를-이용한-api-서비스-배포-12/](https://blog.algopie.com/aws/aws-lambda%eb%a5%bc-%ec%9d%b4%ec%9a%a9%ed%95%9c-api-%ec%84%9c%eb%b9%84%ec%8a%a4-%eb%b0%b0%ed%8f%ac-12/)
- [http://labs.brandi.co.kr/2018/07/31/kwakjs.html](http://labs.brandi.co.kr/2018/07/31/kwakjs.html)

## 7.(DEBUG) cloud watch 연동

- [https://aws.amazon.com/ko/premiumsupport/knowledge-center/api-gateway-cloudwatch-logs/](https://aws.amazon.com/ko/premiumsupport/knowledge-center/api-gateway-cloudwatch-logs/)

## 8. 문제해결

- Debug는 환경설정이 필요한데,
    - (API gateway) API call check 은 포스트맨으로 진행
    - API gateway 이슈는 AWS 콘솔 설정에서 해결
    - 코드 이슈(python & lambda)는 python에서 logging 남기기

### 8.1 파라미터 변경

- 그냥 body (or data)로 object를 줄때는 편했는데 request param으로 주려니 문제 발생
    - lambda proxy integration 하면 해결(파라미터 queryStringParameters)
    - 하면 json으로 받음
- [https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-method-settings-method-request.html#api-gateway-proxy-resource](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-method-settings-method-request.html#api-gateway-proxy-resource)
- 여러가지가 이슈가 있었는데 python script에서 debug 찍는게 제일 편함
- (사전에 cloudwatch 연동 필요)
- [https://sanghaklee.tistory.com/57](https://sanghaklee.tistory.com/57)

### 8.2 CORS 이슈

- 프론트에서 별도로 보내주는 CORS가 다를시에 문제 발생
- CORS란?
    - [https://hannut91.github.io/blogs/infra/cors](https://hannut91.github.io/blogs/infra/cors)
- API Gateway에서 `작업` → `CORS 활성화`
    - 후 꼭 `API를 새로 배포`해야 반영됨

<a href="https://imgbb.com/"><img src="https://i.ibb.co/pRHJxX1/Untitled-2.png" alt="Untitled-2" border="0"></a>

### 8.3 responce time 이슈

- api gateway 최대 대기시간은 30000ms (약 30초)
- 따라서 응답이 30초안에 떨어져야함    
- 이래서 경량화가 필요한가 봄

### 8.4 (체크 필요) URL encoding issue

- 인코딩은 API GATEWAY 레벨에서 알아서 인코딩 해줌 (Good)

다음 블로그를 reference로 참고함

[https://github.com/philschmid/serverless-bert-huggingface-aws-lambda-docker](https://github.com/philschmid/serverless-bert-huggingface-aws-lambda-docker)

[https://aws.amazon.com/ko/blogs/machine-learning/using-container-images-to-run-pytorch-models-in-aws-lambda/](https://aws.amazon.com/ko/blogs/machine-learning/using-container-images-to-run-pytorch-models-in-aws-lambda/)

서버리스 api 참고 : [https://www.serverless.com/framework/docs/providers/aws/cli-reference/create/](https://www.serverless.com/framework/docs/providers/aws/cli-reference/create/)