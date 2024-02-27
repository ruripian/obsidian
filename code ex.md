```python
import datetime as datetime
import zipfile
import os
from datetime import datetime
import pytz

  

def compress_and_delete(folder_path):

# 현재 날짜를 얻습니다.

current_date = datetime.datetime.now()

# 압축 파일 이름 설정

zip_file_name = f"pig_data_{current_date.day}.zip"

with zipfile.ZipFile(zip_file_name, 'w') as zipf:

for foldername, subfolders, filenames in os.walk(folder_path):

for filename in filenames:

file_path = os.path.join(foldername, filename)

zipf.write(file_path, os.path.relpath(file_path, folder_path))

  

# 폴더 내의 모든 파일 삭제

for foldername, subfolders, filenames in os.walk(folder_path):

for filename in filenames:

file_path = os.path.join(foldername, filename)

os.remove(file_path)

for subfolder in subfolders:

subfolder_path = os.path.join(foldername, subfolder)

os.rmdir(subfolder_path)

  

# # 특정 폴더의 경로 설정 (수정 필요)

# folder_to_compress = "os.getcwd()"

  

# # 압축하고 삭제하는 함수 호출

# compress_and_delete(folder_to_compress)

  

#######################################################

# 대한민국 표준시 시간대 객체 생성

ktc_timezone = pytz.timezone('Asia/Seoul')

  

# 현재 UTC 시간을 얻기

current_time_utc = datetime.utcnow()

  

# 현재 시간을 대한민국 표준시 시간대로 변환

current_time_ktc = pytz.utc.localize(current_time_utc).astimezone(ktc_timezone)

  

# 출력

print("현재 KTC 시간:", current_time_ktc.day)

date = current_time_ktc.min

if date != current_time_ktc.min:

b = 2

```