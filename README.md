# terraform

## Overview
### Architecture

![Architecture](architecture.png)
```
이 문서에서는 아래와 같은 AWS의 서비스를 활용하여 CI/CD 환경을 구성합니다. 에플리케이션을 서버는 ECS(Elastic Container Service) Fargate를 활용하겠습니다.

형상 관리: CodeCommit. Git 기반으로 Github와 사용방법이 동일함
빌드: CodeBuild. 애플리케이션 빌드 및 패키징(Maven, NPM 등), Docker 이미지 빌드 및 ECR(Elastic Container Registry)에 Push
배포: CodeDeploy. Rolling Update 또는 Blue/Green 방식의 배포 지원함
파이프라인: CodePipeline. CodeCommit, CodeBuild, CodeDeploy를 파이프라인으로 관리
이미지 저장소: ECR(Elastic Container Registry)
애플리케이션 서버: ECS(Elastic Container Service) Fargate. Fargate는 ECS용 EC2가 없는 완전 관리형 컨테이너 서비스
알림: SNS(Simple Notification Service), Lambda를 활용
CI/CD 전체적인 아키텍처는 아래와 같습니다.

사용자(개발자, 운영자 등) 개발한 코드를 CodeCommit에 반영합니다.
CodeCommit에서 코드 변화를 감지하고 CloudWathch를 통해 CodeBuild에 이벤트를 전달합니다.
CodeBuild에서 애플리케이션 빌드 및 패키징을, Docker 이미지를 빌드, 그리고 빌드한 이미지를 ECR에 Push합니다. Build가 완료되면 CodeDeploy로 이벤트를 전달합니다.
CodeDeploy에서 빌드된 이미지를 활용하여 ECS에 Rolling Update 또는 Blue/Green 방식으로 배포합니다.
CodePipeline의 대시보드를 활용해서 코드를 통합, 빌드하고 배포하는 파이프라인을 구성할 수 있습니다.
CodeDeploy에 SNS, Lambda를 연동해서 Slack등의 Chat 서비스로 배포 과정을 전달할 수 있습니다.
```