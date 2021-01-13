# AWS Lambda
## Cold Start Performance
### Colde Start 해결방법
* ColudWatch를 통해 지속적으로 호출(비용은 감수해야함)
* Route 53(DNS 웹서비스)에서 Health Check를 람다 함수 API Gateway Endpoint로 설정(근데 호출이 많을 때도 Cold Start 발생됨)

-----
### 성능 개선 History
* [updates in Lambda’s execution environment](https://aws.amazon.com/ko/blogs/compute/updated-timeframe-for-the-upcoming-aws-lambda-and-aws-lambdaedge-execution-environment-update/)
* [major improvements in VPC networking](https://aws.amazon.com/ko/blogs/compute/announcing-improved-vpc-networking-for-aws-lambda-functions/)

## Reference
* [How to Improve AWS Lambda Cold Start Performance](https://lumigo.io/aws-lambda-performance-optimization/how-to-improve-aws-lambda-cold-start-performance/)
* [aws-lambda를-시작하기-전-알았으면-좋았을것들](https://medium.com/harrythegreat/aws-lambda%EB%A5%BC-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-%EC%A0%84-%EC%95%8C%EC%95%98%EC%9C%BC%EB%A9%B4-%EC%A2%8B%EC%95%98%EC%9D%84%EA%B2%83%EB%93%A4-788bd3b3bdd2)
* [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)