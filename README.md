# Описание

## .sonarqube-ci.yml

Запуск сканера SonarQube.

```gitlab
include:
  - project: 'devops/gitlab-ci'
    file: '/.sonarqube-ci.yml'

...

sonarqube on merge request:
  extends: .sonarqube
  script:
    - echo "commands..."
    - !reference [.on_merge_request, script]
  rules:
      - if: $CI_MERGE_REQUEST_IID

sonarqube on tag:
  variables:
    SONAR_SCANNER_OPTS: "-Xmx16g"
  extends:
    - .sonarqube
    - .on_tag
  rules:
    - if: $CI_COMMIT_TAG

```

При необходимости переопределить переменные:

`SONAR_SCANNER_OPTS` - параметр запуска сканера;
`PROJECT_VERSION` - определение версии сканируемого проекта;

### Примеры

#### xml

Переопределение `PROJECT_VERSION` при merge request и увеличение памяти сканера при установке tag для выгрузки файлов из 1с в xml.

```gitlab
sonarqube on merge request:
  extends: .sonarqube
  script:
    - export PROJECT_VERSION="$(grep -oPm1 "(?<=<VERSION>)[^<]+" $PROJECT_SRC/VERSION)"
    - !reference [.on_merge_request, script]
  rules:
      - if: $CI_MERGE_REQUEST_IID

sonarqube on tag:
  variables:
    SONAR_SCANNER_OPTS: "-Xmx16g"
  extends:
    - .sonarqube
    - .on_tag
  rules:
    - if: $CI_COMMIT_TAG

```

#### edt

Переопределение `PROJECT_VERSION` для merge request проектов edt.

```gitlab
sonarqube on merge request:
  extends: .sonarqube
  script:
    - export PROJECT_VERSION=$(grep -oPm1 "(?<=<version>)[^<]+" $EDT_PROJECT_DIR/src/Configuration/Configuration.mdo)
    - !reference [.on_merge_request, script]
  rules:
      - if: $CI_MERGE_REQUEST_IID

sonarqube on tag:
  extends:
    - .sonarqube
    - .on_tag
  rules:
    - if: $CI_COMMIT_TAG

```
