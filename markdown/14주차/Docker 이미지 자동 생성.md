<h1>Docker 이미지 자동 생성</h1>

1. 준비사항

   * Docker 배포 마친 repo

   * Github 계정

     

2. 자신의 github repo로 이동

3. Actions 버튼 선택

   * Setup a workflow yourself

   * 이름  "docker-publish.yml"로 설정 후 아래 내용 작성

```
name: Docker Publish

on:
 push:
   branches: [ main ]

 # Allows you to run this workflow manually from the Actions tab
 workflow_dispatch:

jobs:
 build:
   runs-on: ubuntu-latest

   steps:
     - uses: actions/checkout@v2

     - name: Docker Build
       run: docker build -t ${{ secrets.DOCKER_USERNAME }}/my-app .

     - name: Docker Push
       run: |
         docker login -u ${{ secrets.DOCKER_USERNAME }} -p ${{ secrets.DOCKER_PASSWORD }}
         docker push ${{ secrets.DOCKER_USERNAME }}/my-app
```



* Start commit 커밋

  * Settings > Secrets
  * New Repository Secret 생성

  * DOCKER_USERNAME , DOCKER_PASSWORD 각각 저장