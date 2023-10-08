## GitHub Actions

- GitHub Actions은 빌드, 테스트 및 배포 파이프라인을 자동화할 수 있는 CI/CD 플랫폼
- GitHub 레포지토리에서 손쉽게 CI/CD 결과를 확인하고 관리
- YAML 포맷을 사용하여 가독성이 높고, 이미 구현되어 있는 수많은 액션을 활용하여 간단하게 CI/CD 플로우를 작성할 수 있다.

## Github Actions 적용하기

### workflows

- '작업의 흐름'으로, 특정한 목적을 위한 일련의 실행 트리거, 환경, 기능들을 모두 포함
- 하나 이상의 job으로 구성되어 있으며 push나 pull request와 같은 이벤트에 의해 실행될 수 있다.
- 빌드, 테스트, 배포 등 역할에 맞는 workflow를 추가할 수 있고, .github/workflows 디렉토리에 YAML 형태로 저장

아래 예시에서는 main, develop 브랜치에 커밋이 푸시되거나, PR이 만들어지고 동기화될 때 워크플로가 실행된다.

```yml
on:
  push:
    branches:
      - main
      - develop
  pull_request:
```

### job

- 워크플로가 실행되면 수행할 작업을 정의
- 기본적으로 하나의 workflow 내의 여러 job은 독립적으로 실행되지만, 필요에 따라 의존 관계를 설정하여 순서를 지정해줄 수 있다.
- 필수적으로 정의해야 할 속성은 **runs-on**과 **steps**이다.
- `runs-on` : 어떤 환경에서 실행할지 적는다. Windows, Ubuntu, Mac을 지원
- `steps`: 실제로 실행할 단계들을 적는다. 각각의 step은 독립적인 프로세스를 가진다.
  - `uses` : 해당 step에서 사용할 액션 `{owner}/{repo}@{ref|version}`의 형태
  - `name`: 실행시 표시될 이름
  - `run`: shell을 이용하여 커맨드 라인을 실행
  - `env` : 해당 job에서 사용할 환경 변수를 key/value의 형태로 설정
  - `with` : 해당 action에 의해 정의되는 input 파라미터. key/value 페어로 되어 있다.

```yml
jobs:
  build:

    runs-on: ubuntu-latest
    env:
      working-directory: ./

    steps:
      - uses: actions/checkout@v3

      - name: Set up JDK 17
        uses: actions/setup-java@v1
        with:
          java-version: 17
```

위 예시에서는 build라는 job이 하나 존재한다. 이 job은 ubuntu-latest 러너에서 실행되며, actions/checkout@v3 액션과 actions/setup-java@v1 액션을 차례대로 실행한다.

### secrets 환경 변수 등록하기

![img](https://github.com/dilmah0203/TIL/blob/main/Image/Github%20Actions.png)

- api키나, 각종 password 등의 민감한 데이터들은 보안을 이유로 github에 직접 업로드하지 않고 github secrets 기능을 사용해 관리한다.
- Name에는 변수명, Secret에는 값을 입력한다.
- 이렇게 저장된 값은 workflow에서 ${{secrets.VARIABLE_NAME}} 형태로 접근이 가능하다.

```yml
      - name: Build with Gradle
        env:
          KAKAO_SECRET: ${{ secrets.KAKAO_SECRET }}
        run: ./gradlew build

      - name: Run Tests
        env:
          KAKAO_SECRET: ${{ secrets.KAKAO_SECRET }}
        run: ./gradlew test
```










