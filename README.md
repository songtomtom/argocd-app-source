# argocd-app-source

Argo CD GitOps 예제의 **애플리케이션 소스** 저장소입니다. Kubernetes 매니페스트는 [argocd-app-manifests](https://github.com/songtomtom/argocd-app-manifests) 에 따로 둡니다.

블로그 글

1. [Minikube에서 Argo CD 시작하기](https://songtomtom.github.io/blog/argocd-minikube-getting-started)
2. [GitHub를 활용한 GitOps 구현하기](https://songtomtom.github.io/blog/argocd-github-gitops)

## 흐름

```
이 저장소에 push
  └─▶ GitHub Actions: Docker 이미지 빌드 → Docker Hub push
        └─▶ argocd-app-manifests 의 deployment.yaml 이미지 태그 갱신 커밋
              └─▶ Argo CD 가 변경을 감지해 클러스터에 배포
```

## 구성

- `app.py`: Flask 서버. `/` 에서 인사말을 돌려준다.
- `Dockerfile`: python:3.9-slim 기반 이미지
- `.github/workflows/build_and_push.yaml`: 이미지 빌드·푸시와 매니페스트 갱신

## 필요한 시크릿

| 이름 | 용도 |
|---|---|
| `DOCKER_USERNAME`, `DOCKER_PASSWORD` | Docker Hub 로그인 |
| `USER_CHECKOUT_TOKEN` | 매니페스트 저장소에 커밋할 수 있는 GitHub 토큰 (repo 권한) |

## 로컬 실행

```bash
pip install -r requirements.txt
python app.py
# http://127.0.0.1:5001
```
