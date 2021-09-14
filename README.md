# Описание

## .sonarqube-ci.yml

Запуск сканера SonarQube.

```gitlab
include:
  - project: 'devops/gitlab-ci'
    file: '/.sonarqube-ci.yml'
```

При необходимости переопределить параметры:

```gitlab
SONAR_SCANNER_OPTS: "-Xmx4g"

.get_project_version: &get_project_version
  - export PROJECT_VERSION="1.0"
```

`SONAR_SCANNER_OPTS` - параметр запуска сканера;

`PROJECT_VERSION` - определение версии сканируемого проекта;

### Примеры

#### edt

```gitlab
.get_project_version: &get_project_version
  - export PROJECT_VERSION=$(grep -oPm1 "(?<=<version>)[^<]+" $EDT_PROJECT_DIR/src/Configuration/Configuration.mdo)
```

#### xml

```gitlab
.get_project_version: &get_project_version
  - export PROJECT_VERSION="${MAJOR}.$(grep -oPm1 "(?<=<VERSION>)[^<]+" ${PATH_SRC}VERSION)"
```