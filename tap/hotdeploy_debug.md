1. Visual Studio Code Plugin 설치

    아래의 경로에 있는 파일을 다운로드 합니다.

    [tanzu-vscode-extension-0.11.0.vsix 다운로드]
(https://onevmw.sharepoint.com/:u:/r/teams/TAPHOLWorkshop/Shared%20Documents/General/tap1.3.0/tanzu-vscode-extension-0.11.0.vsix?csf=1&web=1&e=0IMHR3)

    Visual Studio Code를 실행해서 VSIX를 설치합니다.
    ![](./images/vs1.png)
    ![](./images/vs2.png)
    ![](./images/vs3.png)
2. VS code 설정

    ![](./images/vs4.png)

3. workload 실행
    ![](./images/workload1.png)

4. Live Update

    Live Update 기능을 시작합니다.
        ![](./images/liveupdate1.png)
    페이지가 정상적으로 열리는지 확인합니다.
    소스를 수정하고 변경사항이 바로 적용이 되는지 확인합니다. 10초 이내에 반영이 되어야 합니다.

5. Debug

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
    
    

