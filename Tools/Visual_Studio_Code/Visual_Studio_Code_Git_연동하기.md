# Visual Studio Code + Git 연동하기

### Step 1. [Visual Studio Download and Install](https://code.visualstudio.com/)

### Step 2. [Git-SCM Download and Install](https://git-scm.com/download)

### Step 3. Git Local 설정

* File > Open Folder...
* Visual Studio Code Left Menu > Source Control (Ctrl + Shift + G) <br/>
  Initialize Repository icon click
* Pick workspace folder to initialize git repo in <br/>
  ex). react-todo-list c:\DEV\React\react-todo-list
* Source Commit.

### Step 4. Local Repository and Github 연동

* [github.com 접속](https://github.com/)
* New repository > repo link copy
* Visual Studio Code Terminal Open (Ctrl + `)
* run command <br/>
`git remote add origin https://github.com/maybe0725/react-router-tutorial.git`
* 연동완료
* push

### Etc. 기존 레포지토리 remote 제거

`git remote remove origin`

### Etc. 이미 존재하는 Github의 레포지토리와 연동

* move the workspace project directory
* Visual Studio Code Terminal Open (Ctrl + `)
* run git clone repository link <br/>
`git clone https://github.com/maybe0725/react-router-tutorial.git`

<br/>
<br/>

참조
> https://evols-atirev.tistory.com/14
