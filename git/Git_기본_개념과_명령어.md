# Git 주요 명령어 정리 

1. git 초기화   
``` git init ```
* .git이라는 숨겨진 폴더가 만들어지고 해당 폴더가 로컬 저장소가 된다.
* 한 폴더에 하나의 로컬 저장소만 유지해야한다. 

2. 커밋 원하는 파일 선택하기  
``` git add README.md ```
* 스테이지에 올리기 

3. 커밋 메세지 달고 커밋하기   
``` git commit -m "파일 추가" ```

4. 생성한 커밋 보기   
``` git log ```
 
5. 로컬 저장소에 원격 저장소 연결   
``` git remote add origin https://github.com/아이디/이름.git ```
 
6. 푸시(push)하기   
   ``` git push orgin 브랜치명  ```

7. 클론(clone)하기
``` 
원하는 폴더로 이동
git clone https://github.com/아이디/이름/git . 
```
* 뒤에 점을 찍어주지 않으면 새폴더를 생성해서 받는다. 

8. 풀(pull)하기    
``` git pull origin 브랜치명  ```

9. git 전역 사용자 설정
``` 
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com 
```

10. 풀 리퀘스트(pull request) : 원격저장소에 머지 요청하기
* 어떤 변경을 했는지 제목과 내용에 작성 
* 오픈소스에 PR보낼때 기여 안내문서 참고
* 팀프로젝트라면 최대한 직접 머지하는건 피하고 모든 머지를 풀 리퀘스틑 통해하기 

11. amend
* 방금 만든 커밋에 추가하기
* 소스트리 amend 방법 
    1. 커밋 클릭하고 오른쪽 상단 모서리에 커밋옵션 클릭
    2. 마지막 커밋 수정 클릭
    3. 강제 푸시하기 

12. stash
* 커밋 전에 스테이징에 올려놨는데 다른 브랜치로 가야할때 변경사항을 잠시 저장할 수 있다.

13. reset
* 옛날 커밋으로 브랜치를 되돌리고 싶을 때 
* 소스트리 reset 방법
    1. 커밋 이력 에서 오른쪽 버튼 클릭
    2. master를 이 커밋 이력으로 되돌리기 클릭
    3. 강제 푸시하기 
        * 강제 푸시 옵션
            * mixed - 변경 있었던걸 남기고 싶을 때 씀, uncommitted changes 로 남는다.
            * hard는 반대로 남지 않는다. 
    
14. revert
* 특정 커밋을 되돌리고 새로운 커밋을 생성한다.
* reset은 다른 사람들의 히스토리에 영향을 준다. revert는 영향을 주지 않고 변경사항을 되돌릴 수 있다. 

15. cherry-pick
* 특정 커밋 하나만 떼서 다른 브랜치에 붙이고 싶을 때 
* 소스트리 cherry-pick 방법
    1. 커밋을 붙이고 싶은 브랜치로 이동
    2. 원하는 커밋에서 오른쪽 버튼 클릭
    3. 체리픽 클릭 


### 브랜치(branch) 
* 기본적으로 master 브랜치 생성 
* 개발자들끼리 서로 다른 변경사항이 생기면 브랜치를 생성함으로서 여러 줄로 커밋을 쌓을 수 있다. 
* HEAD : 내가 지금 작업하는 로컬 브랜치를 가리킴 

1. 브랜치 만들기
   ``` git branch 브랜치명  ```
   
2. 브랜치 이동
   ``` git checkout 브랜치명  ```
* HEAD가 옮겨간다. 

3. 머지(merge)
   ```
   master 브랜치로 HEAD가 옮겨진 상태에서 
   git merge 브랜치명  
   ```
* HEAD는 merge한 브랜치로 옮겨간다.
* 머지 후 같은 곳을 고쳐주어서 충돌이 나게되면 수동으로 수정후 커밋하면된다. 

> 참고
> * 인프런강의 - 팀 개발을 위한 Git, GitHub 입문 기술를 듣고 작성한 글입니다.
> * [강의 링크] [link]

[link]: https://www.inflearn.com/course/%ED%8C%80%EA%B0%9C%EB%B0%9C-%EA%B9%83-%EA%B9%83%ED%97%88%EB%B8%8C/dashboard
