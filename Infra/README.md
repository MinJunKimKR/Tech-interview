# AWS

## CloudFront

## ALB

## VPC

### 리전의 a,b,c,d가 의미하는것은?

## Route53

### CNAME이란?

## Lambda

### 정의

### warm start, cold start란?

    람다는 처음 호출한다면 Cold Start를 하게됩니다. 이 경우 초기 지연시간이 발생합니다.
    그리고 Warm 상태를 유지하며 이때의 경우는 람다를 다시 호출해도 지연시간이 발생하지 않고 빠르게 응답하게됩니다.
    그러다 약 5분여 이상 호출이 되지 않는다면 Cold상태로 바뀌고 다시 호출될 경우 지연시간을 가지고 Cold Start를 하게됩니다.

#### 해결방법은?

Cloudwatch를 이용해서 5분마다 지속적으로 호출을 해주면된다.

### 왜 사용하는가?

### Serverless의 개념에 대해

### EC2와 비교해서 장,단점

## EC2

## SQS

## SNS

# Docker

### EC2와 비교해서 장,단점

# Kubernetes
