# aws-lamdbda

## 1. AWS Lamdbda란 [(AWS Lambda란?)](https://www.44bits.io/ko/keyword/aws-lambda)
> AWS Lamdbda란 아마존 웹 서비스에서 제공하는 서버리스 컴퓨팅 서비스입니다. 별도의 서버 셋업 없이 곧바로 코드를 실행시켜주는 서비스를 의미하며, 고정 비용 없이 사용 시간에 대해서만 비용이 발생합니다. 

현재 AWS Lamdbda의 경우 **C#, Golang, Java, JavaScript, Python, Rudy**를 공식적으로 지원하고 있습니다. 또한 Lambda Runtime에서는 **커스텀 이미지**를 사용하는 것도 가능합니다.

람다는 기본적으로 이벤트 드라이븐 방식으로 동작합니다. API Gateway, S3, DynamoDB, SNS 등 다양한 트리거를 통해 동작할 수 있습니다.

## 2. Lambda 지원 언어 및 비교

### 1.Node.js runtime [(Node.js를 사용하여 Lambda 함수 빌드)](https://docs.aws.amazon.com/ko_kr/lambda/latest/dg/lambda-nodejs.html)
- 현재 지원 가능 버전 Node.js 18,  Node.js 16, Node.js 14, Node.js 12(사용중단 1단계)

### 2.Python runtime [(Python을 사용하여 Lambda 함수 빌드)](https://docs.aws.amazon.com/ko_kr/lambda/latest/dg/lambda-python.html)
- 현재 지원 가능 버전 Python 3.10, Python 3.9, Python 3.8, Python 3.7
### 3.Java runtime [(Java를 사용하여 Lambda 함수 빌드)](https://docs.aws.amazon.com/ko_kr/lambda/latest/dg/lambda-java.html)
- 현재 지원 가능 버전 Java 11, Java 8
### 4.사용자 지정 runtime [(사용자 지정 Lambda 런타임)](https://docs.aws.amazon.com/ko_kr/lambda/latest/dg/runtimes-custom.html)

### 각 언어별 비교

- [AWS Lambda를 시작하기 전에 알았으면 좋았을 것들](https://medium.com/harrythegreat/aws-lambda%EB%A5%BC-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-%EC%A0%84-%EC%95%8C%EC%95%98%EC%9C%BC%EB%A9%B4-%EC%A2%8B%EC%95%98%EC%9D%84%EA%B2%83%EB%93%A4-788bd3b3bdd2)
- [람다 성능 개선 전략 4가지](https://inpa.tistory.com/entry/AWS-%F0%9F%93%9A-%EB%9E%8C%EB%8B%A4-%EC%84%B1%EB%8A%A5-%EA%B0%9C%EC%84%A0-Cold-Start-%ED%95%B4%EA%B2%B0)
> 사용자의 요청이 들어왔을 때 람다 함수를 실행하기 위한 부수적인 준비 세팅이 필요하고 이로인해 코드 딜레이가 발생하게 되는데 이것을 Cold Start라 합니다.

python과 노드의 경우 상당히 빠른 Cold start time을 가지게 되고 그에 반해 자바는 비교적 느린 시작을 알 수 있습니다.

(Cold Start 이후 성능의 경우엔 Java의 성능이 좋다고 합니다.)

### Node.js VS Python

- 실제 소스 실행 속도를 비교한 채널들이 다수 존재 합니다.
- 일반적인 실행 속도만을 비교하면 python의 압승?을 이야기 하는 채널들이 다수 존재하는 것 같습니다.
  - [AWS Lambda Python vs. Node.js performance benchmark](https://antonputra.com/python/python-vs-nodejs-benchmark/)
- 일반적인 속도만 비교하는 것이 아니라 다양한 요소를 비교한 글이 있더라구요.
- [AWS Lambda – Node.js vs. Python for serverless](https://betterdev.blog/aws-lambda-runtime-nodejs-python/)
  - 이 글에서 람다에 사용하기 적합한 언어를 비교하는 데에는 몇가지 **전제조건**이 있습니다.
    1. **특정한 기능을 가지고 있는 경우** 
    가령 람다를 이용하여 머신러닝을 할 경우 굳이 node를 사용할 필요는 없겠지요.
    2. **조직에서 사용하는 친숙한 개발 언어가 있을 경우**
    이미 개발 조직이 Java에 익숙하지만 람다의 성능 차이로 인해 python이나 node를 사용할 정돈 아니라고 말합니다.

  - 여기서 말하는 비교에는 몇 가지의 요소가 들어가게 됩니다.
    1. 번들크기
       - 번들 크기의 경우 250MB를 넘게 되면 Docker 이미지로 빌드를 해야한다고 합니다.(Docker를 사용하게 될 경우 zip파일의 3배 정도의 성능 저하가 발생한다고 말합니다..)
       - 실제 AWS Lambda Cold Start는 큰 문제가 아니지만, 번들 크기가 Cold Start에 영향을 미칠 수 있다고 합니다.
       - Node의 경우 다양한 번들러를 사용하여 소스와 라이브러리를 축소하여 패키지로 묶는 것이 가능합니다.
       - Node의 경우 사용하지않는 소스의 경우 zip파일에 포함이 되지 않습니다.
    2. 함수 번들링 용이성
       - Node.js 를 사용하면 dependencies를 한번 설치하는 것으로 끝이 나기때문에 Docker가 따로 필요하지 않다고 합니다.
    3. MonoRepo 지원
       - 일반적으로 MonoRepo에 대한 지원을 Node.js는 하고 있지만 python의 경우는 불가능 하다고 합니다.
  - 3줄 요약
    - Node 패키징 시 size가 더 작다
    - Node의 경우 Docker가 필요하지 않고 구축 배포가 더 빠르다.
    - Node는 serverless 프로젝트에서 일반적으로 사용되는 MonoRepo를 지원한다.

## 3.Lambda Trigger

### 1.API Gateway [(Amazon API Gateway에서 AWS Lambda 사용)](https://docs.aws.amazon.com/ko_kr/lambda/latest/dg/services-apigateway.html)
- 동기적으로 함수를 호출합니다.

- Http 엔드포인트가 사용자에게 제공합니다

- API 호출을 자동으로 검증 및 승인 하며 대량 호출을 관리할 수 있습니다.
- request 건당 과금이 청구 됩니다.
### 2.SNS [(Amazon SNS에 AWS Lambda 사용](https://docs.aws.amazon.com/ko_kr/lambda/latest/dg/with-sns.html)
- 비동기적으로 함수가 호출합니다.

- Lambda는 메세지 대기열에 추가하고 재시도 처리를 합니다.

- AWS 하나의 계정에서 Lambda를 사용하고 별도의 AWS 계정에서도 구독 할 수 있습니다.
- byte 수를 통해 과금이 청구 됩니다.
