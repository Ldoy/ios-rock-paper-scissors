## iOS 커리어 스타터 캠프

### 묵찌빠 프로젝트 저장소

- 이 저장소를 자신의 저장소로 fork하여 프로젝트를 진행합니다


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
