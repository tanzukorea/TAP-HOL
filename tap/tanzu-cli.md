## Tanzu CLI 설치하기
윈도우즈 이외에는 아래의 문서의 Guide를 참고하시기 바랍니다.
https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.4/tap/install-tanzu-cli.html#cli-and-plugin

### tanzu cli download 
tanzu net 에서 tanzu cli를 다운로드 합니다.
https://network.pivotal.io/products/tanzu-application-platform/

### Tanzu CLI 설치: Windows 
1. Windows 파일 브라우저를 엽니다.

2. 로컬 시스템에 Program Files\tanzu 디렉토리를 생성합니다.

3. 다운로드 디렉토리에서 tanzu-framework-windows-amd64-v0.25.4.5.zip 파일을 마우스 오른쪽 버튼으로 클릭하고 Extract All… 메뉴 항목을 선택한 다음 Files are extract to this directory에 C:\Program files\tanzu를 입력합니다. : 텍스트 상자를 클릭하고 추출을 클릭합니다.

4. Program Files\tanzu 디렉터리에서 실행 파일을 다음 위치로 이동하고 이름을 바꿉니다.

5. Program Files\tanzu\cli\core\v0.25.4\tanzu-core-windows_amd64.exe
  --> Program Files\tanzu\tanzu.exe
6. Program Files 디렉터리에서 tanzu 디렉터리를 마우스 오른쪽 버튼으로 클릭하고 속성 > 보안을 선택합니다.
  사용자 계정에 전체 제어 권한이 있는지 확인하십시오.
7. Windows 검색을 사용하여 env를 검색하고 시스템 환경 변수 편집을 선택한 다음 대화 상자 오른쪽 하단의 환경 변수를 클릭합니다.
8. 시스템 변수 아래에서 경로 행을 찾아 선택하고 편집을 클릭합니다.
9. 새로 만들기를 클릭하고 경로 값을 입력한 다음 확인을 클릭합니다.
    참고: 경로 값은 tanzu.exe를 포함하지 않아야 합니다. 예: C:\Program Files\tanzu

10. 시스템 변수 섹션 다음에 새로 만들기를 클릭하고 변수 값이 true인 TANZU_CLI_NO_INIT라는 새 환경 변수를 추가하고 확인을 클릭합니다.

11. Windows 검색을 사용하여 cmd를 검색하고 명령 프롬프트를 선택하여 명령줄 터미널을 엽니다.

12. 다음을 실행하여 Tanzu CLI 설치를 확인합니다.

```
    tanzu version
```

### Tanzu CLI 플러그인 설치/업데이트 진행

Tanzu CLI 플러그인 설치/업데이트
터미널에서 Tanzu CLI 플러그인을 설치하거나 업데이트하려면 다음 단계를 따르십시오.

다음을 실행하여 $HOME/tanzu 디렉터리(Linux 또는 macOS의 경우) 또는 Program Files\tanzu 디렉터리(Windows의 경우)에서 플러그인을 설치합니다.
```
tanzu plugin install --local cli all
```
다음을 실행하여 플러그인을 설치했는지 확인하십시오.
```
tanzu plugin list
```
