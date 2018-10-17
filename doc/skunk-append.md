skunk 해시 알고리즘 추가
=======================
skunk 해시 알고리즘에 대응한 기능이 추가되었습니다.

기존의 nomp는 skunk 해시 알고리즘을 지원하지 않기 때문에 package를 새롭게 설치해줘야 합니다. 기존 설정 파일은 그대로 사용할 수 있으므로 백업해 두시면 됩니다.
관련 설정 파일은 다음과 같습니다.
* config.json
* pool_configs/h-dac.json

### 설치 방법
 설치 방법은 다음과 같습니다.
```bash
git clone https://github.com/Hdactech/nomp.git
cd nomp
npm install
```

### 환경 설정
coins/hdac.json 의 내용 중에 "algorithm" 값이 "skunk" 로 되어 있어야 합니다. 관련 내용은 다음과 같습니다.
```bash
"name": "HDAC",
"symbol": "DAC",
"algorithm": "skunk"
//"algorithm": "lyra2rev2"
...
```

### nomp 실행
실행하기 전에 백업해두었던 파일 - config.json, pool_configs/h-dac.json - 을 복구합니다.
실행 방법은 다음과 같습니다.
```bash
node init.js
```
