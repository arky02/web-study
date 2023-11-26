Git에서 branch merge 방법들과 각 방법의 특징을 설명해 주세요.
---
git merge란? git branch를 다른 branch로 병합하는 것. git merge 명령어로 커밋 단위로는 합칠 수 없다.
git branch merge의 방법에는 fast forward merge, commit merge, conflict merge가 있습니다. 

1. Fast-Forward Merge
   가장 기본적인 merge. 현재 브랜치(main)의 head(commit)가 대상 브랜치(feature)의 head(commit)까지로 옮기는 merge이다.

> $ git checkout main</br>
> $ git merge feature

단, fast forward merge는 중간에 변경이 없을 때만 동작한다. 
새로운 feature 브랜치가 main으로부터 분기된 이후 main 브랜치에 새로운 커밋이 올라오지 않았다면 feature 브랜치가 main 보다 최신의 브랜치라고 볼 수 있다.
이 경우 feature에서 작업한 (분기 이후 feature에서 새로 생성된) 커밋들을 main으로 가져와 붙여넣기 해도 된다. => main을 feature로 최신화 함.
이를 Fast-Forward Merge라고 한다. 이는 모든 커밋 기록을 삭제하지 않고 남길 수 있지만 그로인해 커밋 로그가 지저분해 질 수 있다.


2. Squash & Merge
   Squash는 여러개의 커밋을 하나의 커밋으로 합치는 것을 의미한다. feature 브랜치에서 작업한 커밋들을 하나의 원기옥으로 모아 딱 하나의 커밋으로 합치고 지움.
   따라서 Squash&Merge는 병합할 브핸치의 모든 커밋을 하나의 커밋으로 Squash한 새로운 커밋을 base브랜치에 추가하는 방식으로 병합하는 것을 의미한다.

> $ git checkout main</br>
> $ git merge --squash my-branch</br>
> $ git commit -m "squash & merge"</br>

Squash & Merge는 커밋 로그가 깔끔해지고, 각 브랜치에서 한 작업들로 크게크게 커밋이 남아 가독성이 좋아지는 장점이 있지만, 기록이 남지않아 문제가 생기면 찾기 복잡해진다.

 3. Rebase
    Rebase는 말 그대로 머지할 때에 base를 다시 설정한다는 것이다. 원래 feature branch가 main의 A 커밋에서부터 분기되었다고 하면,
    feature branch의 base는 A 커밋이지만, feature 브랜치를 작업하는 동시에 main 브랜치에도 변경사항이 생겨 B,C 커밋이 새로 생겼다고 하면,
    이 때에 main브랜치로 feature 브랜치를 rebase & merge 하고자 하면 feature 브랜치는 main 브랜치의 최신 커밋, 즉 C 커밋으로 base(분기점)를 옮겨
    C 커밋에 붙여서 Merge 된다.

> $ git checkout feature</br>
> $ git rebase main => feature에 연결된 main브랜치로의 base를 re-base 한다.</br>
> $ git checkout main</br>
> $ git merge feature</br>

보기에도 깔끔하고, 버전 관리에 용이하지만 conflict가 일어날 경우 rebase되어 merge 된 커밋들 각각에 하나씩 충돌이 발생하므로 한번에 많은 커밋을 merge 할 경우 conflict가 발생하면
고치기 힘들다. force push를 해야하는 경우가 대부분이다.

+) 언제 어떤 방식을 사용하면 좋을까.
feature → develop 머지
Squash & Merge가 유용하다. feature 브랜치에서 기능을 개발하기 위한 지저분한 커밋 내역을 하나의 커밋으로 묶어 develop 에 병합하면서, develop에는 기능 단위로 커밋이 추가되도록 정리할 수 있다.
또한 feature 브랜치는 develop 브랜치에 병합 후 제거하므로, Merge Commit 을 남길 필요가 없다.

develop → main 머지
main 브랜치는 지금까지 작업한 모든 기능을 배포할 때 병합한다. develop 브랜치를 squash & merge 하게 되면 커밋 이력이 모두 사라져, 특정 기능에서 문제가 생겼을 때 롤백할 수 없게된다. main 브랜치 또한 Merge Commit 을 남길 필요 없다. 따라서 Rebase & Merge 가 적합하다.
