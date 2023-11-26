Git Flow 브랜치 전략에 대해 설명해 주세요.
====

Git Flow는 Vincent Driessen이 그의 블로그에 2010년에 올린 A successful Git branching model 이라는 글이 인기를 끌며 대중적으로 사용되게된 브랜치 전략이다.

Git Flow는 크게 Main 브랜치, Develop 브랜치, Supporting 브랜치로 구분하여 브랜치를 관리한다. 
이때, Supporting 브랜치는 또 다시 Feature 브랜치, Release 브랜치, Hotfix 브랜치로 나뉜다.

Main-Develop-(Supporting)
                  ㅣ
               feature
                  ㅣ
               release
                  ㅣ
                Hotfix

Main 브랜치와, Develop 브랜치는 개발 프로세스 전반에 걸쳐 항상 유지되는 브랜치이다. 
반면, Supporting 브랜치는 필요할 때마다 생성되고, 역할을 다하면 삭제된다. Supporting 브랜치 덕분에 팀이 병렬적으로 업무를 할 수 있게된다.

1. Main Branch
Main 브랜치는 출시 가능한 프로덕션 코드를 모아두는 브랜치이다. Main 브랜치는 프로젝트 시작 시 생성되며, 개발 프로세스 전반에 걸쳐 유지된다. 배포된 각 버전을 Tag를 이용해 표시해둔다.

2. Develop Branch
다음 버전 개발을 위한 코드를 모아두는 브랜치이다. 개발이 완료되면, Main 브랜치로 머지된다.

3. Feature Branch
하나의 기능을 개발하기 위한 브랜치이다. Develop 브랜치에서 생성하며, 기능이 개발 완료되면 다시 Develop 브랜치로 머지된다. 머지할때 주의점은 Fast-Forward로 머지하지 않고, Merge Commit을 생성하며 머지를 해주어야 한다. 이렇게해야 히스토리가 특정 기능 단위로 묶이게 된다.
feature/branch-name 과 같은 형태로 생성한다.

4. Release Branch
소프트웨어 배포를 준비하기 위한 브랜치이다. Develop 브랜치에서 생성하며, 버전 이름 등의 소소한 데이터를 수정하거나 배포전 사소한 버그를 수정하기 위해 사용된다. 배포 준비가 완료되었다면 Main과 Develop 브랜치에 둘다 머지한다. 이때, Main 브랜치에는 태그를 이용하여 버전을 표시한다.
Release 브랜치를 따로 운용함으로써, 배포 업무와 관련없는 팀원들은 병렬적으로 Feature 브랜치에서 이어서 기능을 개발할 수 있게된다.

release/v1.1 과 같은 형태로 생성한다.

5. Hotfix Branch
이미 배포된 버전에 문제가 발생했다면, Hotfix 브랜치를 사용하여 문제를 해결한다. Main 브랜치에서 생성하며, 문제 해결이 완료되면 Main과 Develop 브랜치에 둘다 머지한다.
Release 브랜치와 마찬가지로 Hotfix 브랜치를 따로 운용함으로써, 핫픽스 업무와 관련없는 팀은 병렬적으로 기능 개발을 할 수 있다.

hotfix/v1.0.1 과 같은 형태로 생성한다.

