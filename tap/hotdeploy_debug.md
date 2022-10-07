## 동적 배포와 원격 디버깅
여기서는 간단한 guide만 설명하고 있습니다. 자세한 내용은 아래의 주소에서 확인할 수 있습니다.

https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.2/tap/GUID-vscode-extension-using-the-extension.html


### VS Code 설정하기

1. Visual Studio Code Plugin 설치

    아래의 경로에 있는 파일을 다운로드 합니다.

    [tanzu-vscode-extension-0.11.0.vsix 다운로드]
(https://onevmw.sharepoint.com/:u:/r/teams/TAPHOLWorkshop/Shared%20Documents/General/tap1.3.0/tanzu-vscode-extension-0.11.0.vsix?csf=1&web=1&e=0IMHR3)

    Visual Studio Code를 실행해서 VSIX를 설치합니다.
    ![](./images/vs1.png)
    ![](./images/vs2.png)
    ![](./images/vs3.png)

    설치시 같이 추천하는 아래의 plugin도 설치합니다.
    - Debugger for Java
    - Language Support for Java(™) by Red Hat
    - YAML
    
2. Java 언어 모드 설정
    
    설정화면에서 
    Code > Preferences > Settings Java > Server: Launch Mode 를 Standard로 변경합니다.
    ![](./images/images-vscode-install4.png)

3. Source Image 설정
    VSCode plugin에서는 개발자 PC에서 작성된 소스를 docker image로 압축하여 harbor에 업로드를 하게 됩니다. 그 후에 해당 이미지를 source로 사용하여 TAP에 배포를 하게 됩니다.
    여기서는 소스가 저장될 docker registry인 harbor의 주소를 입력해줍니다.
    
    Code > Preferences > Settings > Extensions > Tanzu Developer Tools 에서 Source Image에 주소를 입력합니다.

    예:) harbor.tanzukr.com/app/sources

    ![](./images/vs4.png)


### 동적 배포와 원격 디버깅
1. 소스 download
    로컬 경로에 자신의 github.com 에 올려져 있는 tanzu-java-web-app을 git clone을 통해 소스를 다운로드 합니다.
    ```
    예:)    git clone https://github.com/wonjoyoo/tanzu-java-web-app
    ```
2. Visual Studio Code 에 '폴더 열기' 메뉴를 클릭하여 위에서 다운로드 받은 폴더를 엽니다.
3. 파일 내용 확인 및 수정
    ```
    .mvn/wrapper/maven-wrapper.properties
    ```
    위 파일을 열고 3번째 줄이 빈줄로 있는 경우 3번째 줄을 없앱니다.

3. workload 실행
    ![](./images/workload1.png)

4. Live Update

    Live Update 기능을 시작합니다.
        ![](./images/liveupdate1.png)
    페이지가 정상적으로 열리는지 확인합니다.
    소스를 수정하고 변경사항이 바로 적용이 되는지 확인합니다. 10초 이내에 반영이 되어야 합니다.

3. Debug

    Debug기능을 이용하기 위해 먼저 마우스로 소스에 break point를 설정하고 Debug Start 를 시작합니다.
    ![](./images/debug.png)

    Tilt 파일에 아래와 같은 권한이 필요하다고 나옵니다. 동일하게 내용을 Tilt 파일 제일 아래부분에 추가시킵니다.
    ![](./images/debug_tilt.png)

    Tilt 파일 수정
    ```
    allow_k8s_contexts('cluster1-admin@cluster1')
    ```
    
    다시 호출해서 workload가 Ready가 되면 페이지를 호출합니다. 페이지가 정상적으로 열리지 않고 pending상태가 되어 있는 것을 확인할 수 있습니다.
    ![](./images/debug1.png)
    VS Code에서 Resume기능이나 한 Step 이동하는 기능으로 Debug를 확인합니다.
    
    

