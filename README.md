## iOS 커리어 스타터 캠프

### 묵찌빠 프로젝트 저장소

- 이 저장소를 자신의 저장소로 fork하여 프로젝트를 진행합니다

1. 왜 add, commit 이후 깃헙 레포에 반영되지 않았을까?
    - 상황 :
        - 로컬 저장소를 먼저 만들지 않고 원격 저장소를 만들고(파일 개수 : 0) 로컬로 clone 해옴
        - config를 확인했는데 오타없이 입력되어 있었음
        - 깃에서 제공하는 아래 코드를 실행 했더니 이전에 했던 것이 커밋이 반영됨!

            ```swift
            echo "# init" >> README.md
            git init
            git add README.md
            git commit -m "first commit"
            git branch -M main
            git remote add origin https://github.com/Ldoy/init.git
            git push -u origin main
            ```

    - 해결

        : 위의 상황과 동일하게 만들고 다시 진행해 봄, 단 내가 생각했던 이유(git push 안한 것)를 반영함 

        - 순서 : 원격저장소 생성 → 로컬에 클론 → 파일 생성 후 깃 add, commit 까지만 진행

            → 원격 저장소에 반영 안되어 있음 → 위의 코드 실행 → 이전에 커밋한 것 까지 모두 반영됨

            - 실험 목적 :  push를 안해서 위와 같은 문제가 있었던 것 같은데 이를 확인해보자!
                1. 원격 저장소 생성(깃헙)
                2. 로컬 저장소에 클론(git clone)
                3. git status를 실행하니 "Your branch is based on 'origin/master', but the upstream is gone" 라는 경고가 뜸
                    - 아직 파일이 한개도 없다라는 뜻 (is gone = is empty)
                4. 무시하고 a.txt 만들고 add-commit-push진행
                5. 정상 반영됨 

            ⇒ git push를 안해서 반영이 안된 것으로 확인됨 

        - 그렇다면 깃헙 제공 코드를 입력했더니 왜 이전에 push 하지 않은것 까지 커밋 된 것일까?
            - git push : 이전의 commit 했던 것들이 반영되기 때문, 위의 코드의 경우 orgin 이라는 원격 저장소에 main 브랜치의 모든 커밋을 반영!
            - 그렇다면 브랜치를 만들고 해당 브랜치에서 add, commit 만 진행하고 다시 main 브랜치로 와서 push를 진행하는 경우엔 어떻게 될까?
                - 실험 진행  ⇒ 반영 안됨! 해당 브랜치에서 push 진행해주기

                    ```jsx
                    ➜  second git:(main) git checkout -b issue-1 //1. 브랜치 만들기 
                    Switched to a new branch 'issue-1'
                    ➜  second git:(issue-1) touch a.txt // 2. 파일 생성
                    ➜  second git:(issue-1) git add a.txt // 3. add, commit
                    ➜  second git:(issue-1) git commit -m "branch-1"
                    On branch issue-1
                    nothing to commit, working tree clean // 의문점 1
                    ➜  second git:(issue-1) git status
                    On branch issue-1
                    nothing to commit, working tree clean
                    ➜  second git:(issue-1) git checkout main //4.main으로 이동
                    Switched to branch 'main'
                    Your branch is up to date with 'origin/main'.
                    ➜  second git:(main) git push -u origin main //5. push
                    Branch 'main' set up to track remote branch 'main' from 'origin'.
                    Everything up-to-date
                    ➜  second git:(main)
                    //결과 : 브랜치 원격 저장소에 반영 안됨 
                    ```

                - 실험 하면선 생긴 의문점 1
                    - "On branch issue-1
                    nothing to commit, working tree clean " 가 생긴 이유
                        1. 나는 브랜치에서 add→commit 만 한 상태이고 push하지 않았기 때문!
                        2. 이를 해결하기 위해선 브랜치가 시작된 지점인 main 브랜치와 merge 를 진행하던가 push진행하기!

2. 공동 작업 시 커밋 제한 오류
    1. 이유 : Contributor로 추가하지 않았기 때문
    2. 방법 : 해당 레포의 주인이 설정에서 contributor에 공동 작업자를 추가해야한다 → 공동 작업자가 초대 수락하면 그 이후부터 각자의 로컬에서 커밋 가능 

    - 그 외의 커밋 관련 오류
        1. 유저의 SSH등록되어있지 않은 경우 (로컬저장소 생성 후 원격 저장소 연결해 줄 때 발생)
            - 해결방법 : [https://zeddios.tistory.com/120](https://zeddios.tistory.com/120)

3. merge 충돌 
    1. 충돌 일어나는 이유

        파일이 다르면 무조건 자동으로 합쳐준다.
        파일이 같아도 수정한 부분이 다르다면 자동으로 합쳐준다.
        (버전관리를 사용하는 정말 중요한 이유중의 하나)
        ⇒ 파일이 같고, 수정한 부분이 같다면 충돌이 발생한다.

    2. 해결 방법
        1. 수동으로 파일 수정

            <<<<<< HEAD 부터 >>>>>>까지가 충돌이 일어난 부분이고

            <<<<<< HEAD 부터 ======까지는 master브랜치의 소스코드

            ======부터 >>>>>> (브랜치이름)까지는 브랜치의 소스코드

            [https://jeong-pro.tistory.com/106](https://jeong-pro.tistory.com/106)

    3. 예방 방법 
        1. 같은 파일(같은 라인)작업할 땐 시간 다르게 작업
        2. master branch를 수시로 가져오기 = push, pull, commit 많이하기


1. 진트가 내 원격저장소를 클론한 후 커밋 이후 pull 이 안됨 
    - 깃 클론하고 나면 git branch 명령어에 main 브랜치만 나오는데(실제는 브랜치 다 있음) 클론이 안 된것으로 생각 → @jint H 진트 이거 시간될 때 알려주세요!

    [https://velog.io/@hyun0310woo/git](https://velog.io/@hyun0310woo/git)

    체크아웃을 알고 있었음

    깃 브랜치 치면 메인밖에 안뜸 → 보이게 함 

**2. pull 하고 나서 commit 내용이 xcode에 반영되지 않음** 

- git pull을 했는데 xcode에 pull로 가져온 changed code는 회색으로 pull이전까지 작업하던 내용 중 일부는 파란색으로 표시됨
    1. 해결하기 위한 시도 : 다시 pull 을 진행했더니 stash 한 후에 pull 하라고 제안해서 진행 ⇒ 정상적으로 화면에 changed conde 가 표시됨 
    2. 찾아본 내용 
        - pull 을 취소하기 위해 찾아봄

            [https://www.devpools.kr/2017/02/05/초보용-git-되돌리기-reset-revert/](https://www.devpools.kr/2017/02/05/%EC%B4%88%EB%B3%B4%EC%9A%A9-git-%EB%90%98%EB%8F%8C%EB%A6%AC%EA%B8%B0-reset-revert/)

    3. **문제 원인** : 원격 저장소에 push한 이후 의도치 않게 수정 된 부분이 생겼고 이 상태에서 pull 하는 경우 merge가 제대로 일어날 수가 없게 됨 
        - [해결 방법](https://stackoverflow.com/questions/12476239/git-stash-and-git-pull)
            1. git stash  진행 : 내가 진행하려는 pull은 fast-forward merge 형태이기 때문에 해당 명령어 사용 → 내가 변경했던 부분이 사라짐
                - git stash pop : fast-forward merge 가 아닌 경우, merge conflict 가 일어나지 않는 이상 working copy 에서 변경했던 부분들을 모두 지움

                    ⇒ git stash list : stash리스트 볼 수 있음!

            2. git pull
    4. 의문점 : 이 명령어가 적용될 수 있었던 이유는 내가 변경한 사항을 commit하지 않았기 때문일까?

**3. 마크다운 이미지 넣기**

- 형식 : ![파일 이름](./깃헙 이미지 파일의 경로 or url)
- 깃헙 저장소에 있는 이미지 파일을 리드미에 추가하기

    ```swift
    //1. 깃 터미널을 열고 해당 브랜치에 이미지가 저장될 폴더 만들기

    git mkdir image

    //2. 깃헙에서 해당 폴더에 파일 업로드 하기

    //3. 로컬 저장소에서 리드미 파일 수정하기 
    git open README.md

    (리드미 파일 아래 내용 추가 후 저장)
    ![파일 이름](./깃헙 이미지 파일의 경로)
    ex :  ![workflow](./imge/workflow.jpg)

    //4. git add, commit, push
    ```

# 😭 느낀점

- 커밋 규칙에 대해 공부하였지만 실제 적용하고 습관화 하는데 시간이 걸릴 것 같다
- 깃 문제가 생길 때 마다 등골이 오싹하다.. 많이 연습하고.. 깃 사용에 대해 설명할 수 있는 사람이 되자!
    - 깃헙 PR 페이지의 리뷰어 항목에 "Still in progress?"라고 써져있는 것을 리뷰어인 kane이 쓴 건 줄 알고 내가 쓴 PR 에 문제가 있는 줄로 착각하고 고치고 다시쓰고를 반복..슬프닷

        ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c1c4c1b2-e047-4128-b12f-4af22683b0da/Screen_Shot_2021-05-27_at_0.30.50.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c1c4c1b2-e047-4128-b12f-4af22683b0da/Screen_Shot_2021-05-27_at_0.30.50.png)


# Issue

문제발생

1. 다른 브랜치에 커밋을 진행

해결방법

1. 체리픽을 이용해 메인 브랜치의 마지막 커밋을 3-doyi로 옮김 
    1. 커밋을 옮기고 싶은 브랜치로 이동
    2. 내가 옮기고 싶은 커밋의 commit code를 복사
    3. git cherry-pick **AAAA** ( 여러개 하는 경우 띄어쓰기로 구분하며 맨 처음 커밋 바로뒤에 ^ 붙임)
    4. 잘못 커밋했던 브랜치로 다시 돌아가서 해당 브랜치의 커밋 삭제 
        1. git checkout **BBBB**
        2. git reset **AAAA**^ —hard
    5. 성공!

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b62353c4-fdba-4c17-b050-aeca090333a5/Screen_Shot_2021-05-27_at_13.22.29.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b62353c4-fdba-4c17-b050-aeca090333a5/Screen_Shot_2021-05-27_at_13.22.29.png)

    [https://hayeon1549.tistory.com/13](https://hayeon1549.tistory.com/13)

2. 3-doyi라는 브랜치에서 분기하여 브랜치를 만들었는데 터미널 git log 를 하면 해당 내용이 나타나지가 않음
    1. 원인 후보 
        1. git 에서는 분기되는 내용이 나오지 않는다
            1. 해결
                - git log
        2.  분기를 잘못했다 
            1. 해결을 어떻게 할 것인가? 
                - rebase 명령어를 사용해서 분기점을 옮기기

    # 고민한 점

    1. filterUserInput을 구현하면서 split을 써야할 지 components를 써야 할 지 고민하였는데 max split이라는 매소드를 이용할 수 있는 점, components보다 더 빠른점을 이유로 해당 함수를 사용하였음 

    ```swift
    func filterUserInput(user num: String?) -> [Int?] {
        
        if let userNums = num {
            let userNumSet: Set = Set(userNums.split(separator: " ", maxSplits: 2).map { Int($0) })
            let basicNum:Set = [1, 2, 3, 4, 5, 6, 7, 8, 9]
            
            let userNumLast = userNumSet.intersection(basicNum).map { $0 }
            
            return userNumLast
        } else {
            print("입력이 잘못되었습니다")
        }
        
        showOption()
    }
    ```

### branch 관리와 commit 단위에 대한 고찰

- 진트와의 논의
    1. main branch는 항상 사용가능한 버전으로 유지
    2. 새로운 기능 또는 함수 추가시에 새로운 브랜치 feature1 생성
    3. feature1 에서 팀원 각자만큼의 branch 생성 (ex: feature1_Tacocat, feature1_jint)
    4. 각자의 브랜치를 로컬에 클론하여 자주 commmit 하며 개발
    5. 각자의 결과물을 종합하여 최상의 버전을 feature1에 merge
    6. feature1을 main에 merge
- 현장에서는 어떻게 하고있을지 궁금하다.





## 문제점 / 고민한 점

1. 프로젝트 코드에서 어떻게 하면 사용자의 입력값을 3개의 숫자로만 구성되어 있도록 값을 저장할 수 있을지에 대해 고민하였다.
2. 어떻게 하면 옵셔널 값을 강제 언래핑하지 않고 처리할 수 있을지 고민하였다
3. guard와 if-let 중 어떤것으로 옵셔녈 값을 처리할 지 고민하였다.
