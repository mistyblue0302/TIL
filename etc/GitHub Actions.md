## GitHub Actions

- GitHub Actions은 빌드, 테스트 및 배포 파이프라인을 자동화할 수 있는 CI/CD 플랫폼
- repository에 대한 모든 pull request를 빌드 및 테스트하는 workflow를 생성
- merge pull request를 서버에 배포할 수 있다.

### Github Actions의 구성 요소

1. **workflows**

- repository에 추가할 수 있는 일련의 자동화된 명령 집합
- 하나 이상의 job으로 구성되어 있으며 push나 pull request와 같은 이벤트에 의해 실행될 수 있다.
- 빌드, 테스트, 배포 등 역할에 맞는 workflow를 추가할 수 있고, .github/workflows 디렉토리에 YAML 형태로 저장

2. **event**

- event란 Workflow를 실행시키는 push, pull request, commit 등의 특정 행동
- [event의 종류](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows)

3. **job**

- 이벤트가 발생하면 실제 실행할 내용
- 기본적으로 하나의 Workflow 내의 여러 Job은 독립적으로 실행되지만, 필요에 따라 의존 관계를 설정하여 순서를 지정해줄 수 있다.
- runs-on : 어떤 환경에서 실행할지 적는다. Windows, Ubuntu, Mac을 지원
- steps: 실제로 실행할 단계들을 적는다.
  - name: 실행시 표시될 이름
  - run: 실제로 컴퓨터상에서 실행할 스크립트
  - if : 해당 step의 실행조건을 명시

4. **actions**

- 자주 반복되는 작업(task)을 수행하는 커스텀 응용 프로그램
- [actions의 종류](https://docs.github.com/en/actions/creating-actions)

5. **runner**

- workflow가 트리거될 때 실행하는 서버
-  각 runner는 1번에 1개의 job을 실행할 수 있다.

