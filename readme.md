# Prepare commit msg

브랜치 이름에서 Jira 티켓 번호 형식 (예: abc-123)을 찾아내어 커밋 메시지에 자동으로 추가합니다.

예를 들어, 브랜치 이름이 feature/abc-123-new-feature인 경우, 커밋 메시지는 ABC-123 [원래 메시지] 형식으로 시작됩니다. 이 스크립트는 Jira 티켓 번호 패턴을 가진 모든 브랜치에 적용됩니다.

## .git/hooks/prepare-commit-msg

1. **`.git/hooks/`** 디렉토리로 이동합니다. 이 디렉토리는 Git 저장소의 루트에 위치합니다.
2. **`prepare-commit-msg`** 파일을 생성하거나 편집합니다. 이 파일이 없다면 새로 만듭니다.
3. 다음 스크립트를 **`prepare-commit-msg`** 파일에 추가합니다:

```shell
#!/bin/sh

# 브랜치 이름 가져오기
BRANCH_NAME=$(git symbolic-ref --short HEAD)
TICKET_NUMBER=""

# 브랜치 이름에서 Jira 티켓 번호 추출 (예: abc-123)
if [[ $BRANCH_NAME =~ ^[a-zA-Z]+-[0-9]+ ]]; then
    TICKET_NUMBER=$(echo $BRANCH_NAME | grep -oE '[a-zA-Z]+-[0-9]+')
fi

# 커밋 메시지에 티켓 번호 추가
if [ -n "$TICKET_NUMBER" ]; then
    echo "$TICKET_NUMBER $(cat $1)" > $1
fi
```

4. 권한을 추가합니다.

```shell
chmod +x .git/hooks/prepare-commit-msg
```
