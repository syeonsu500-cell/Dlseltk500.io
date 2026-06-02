import pandas as pd

file_path_parking = '서울시 전동킥보드 주차구역 현황.csv'

try:
    df_parking = pd.read_csv(file_path_parking, encoding='cp949', engine='python')
    print(f"'{file_path_parking}' 파일 로드 성공!")
    print("\ndf_parking 데이터프레임의 첫 5행:")
    display(df_parking.head())
except FileNotFoundError:
    print(f"오류: '{file_path_parking}' 파일을 찾을 수 없습니다. 파일을 Colab 환경에 업로드해주세요.")
except pd.errors.ParserError as e:
    print(f"오류: '{file_path_parking}' 파일을 파싱하는 데 문제가 발생했습니다: {e}")
    print("엔진을 'python'으로 변경하여 재시도합니다.")
    try:
        df_parking = pd.read_csv(file_path_parking, encoding='cp949', engine='python')
        print(f"'{file_path_parking}' 파일 재로드 성공!")
        print("\ndf_parking 데이터프레임의 첫 5행:")
        display(df_parking.head())
    except Exception as e_inner:
        print(f"재시도 실패: {e_inner}")
except Exception as e:
    print(f"파일 로드 중 알 수 없는 오류 발생: {e}")

import pandas as pd

file_path_parking = '서울시 전동킥보드 주차구역 현황.csv'

try:
    df_parking = pd.read_csv(file_path_parking, encoding='cp949', engine='python')
    print(f"'{file_path_parking}' 파일 로드 성공!")
except FileNotFoundError:
    print(f"오류: '{file_path_parking}' 파일을 찾을 수 없습니다. 파일을 Colab 환경에 업로드해주세요.")
    # Colab에서 파일을 업로드하려면 다음 코드를 사용하세요:
    # from google.colab import files
    # uploaded = files.upload()
    # for fn in uploaded.keys():
    #   print(f'User uploaded file "{fn}" with length {len(uploaded[fn])} bytes')
except pd.errors.ParserError as e:
    print(f"오류: '{file_path_parking}' 파일을 파싱하는 데 문제가 발생했습니다: {e}")
    print("엔진을 'python'으로 변경하여 재시도합니다.")
    try:
        df_parking = pd.read_csv(file_path_parking, encoding='cp949', engine='python')
    except Exception as e_inner:
        print(f"재시도 실패: {e_inner}")
except Exception as e:
    print(f"파일 로드 중 알 수 없는 오류 발생: {e}")

# 데이터 로드 후 df_parking의 첫 몇 행을 확인하여 성공적인 로드 검증
if 'df_parking' in locals() and not df_parking.empty:
    print("\ndf_parking 데이터프레임의 첫 5행:")
    display(df_parking.head())

from IPython.display import display, HTML
import pandas as pd

# --- Add this block to load df for '서울시 전동킥보드 견인 현황.csv' ---
file_path_towing = '/content/서울시 전동킥보드 견인 현황.csv'

try:
    df = pd.read_csv(file_path_towing, encoding='cp949')
except UnicodeDecodeError:
    print("cp949 인코딩으로 파일을 읽는 데 실패했습니다. euc-kr 인코딩을 시도합니다.")
    df = pd.read_csv(file_path_towing, encoding='euc-kr')
# ------------------------------------------------------------------

# 'df_kickboard' 대신 이미 로드된 'df' 변수를 사용합니다.
# '구정보'로 견인 건수의 총합 (즉, 각 구별 견인 발생 횟수)을 구하기
district_tows = df.groupby('구정보').size()
district_tows.name = '견인건수 총합' # 결과 Series에 이름 부여

# 총합이 최소인 구, 최대인 구 찾기
min_tows_district = district_tows.idxmin()
max_tows_district = district_tows.idxmax()

# 평균값과 중간값 계산
mean_tows = district_tows.mean()
median_tows = district_tows.median()

# 평균값에 가장 가까운 구 찾기
# 절대값 차이가 가장 작은 값을 찾음
closest_to_mean_district = (district_tows - mean_tows).abs().idxmin()

# 중간값에 가장 가까운 구 찾기
closest_to_median_district = (district_tows - median_tows).abs().idxmin()

# --- 구별 견인 건수 총합 표 생성 ---
district_tows_df = district_tows.reset_index()
district_tows_df.columns = ['구정보', '견인건수 총합']

# --- 요약 통계 DataFrame 생성 ---
summary_data = {
    '지표': ['최소 견인 건수 구', '최대 견인 건수 구', '평균 견인 건수', '평균값에 가장 가까운 구', '중간값 견인 건수', '중간값에 가장 가까운 구'],
    '값': [
        f"{min_tows_district} ({district_tows.min()}건)",
        f"{max_tows_district} ({district_tows.max()}건)",
        f"{mean_tows:.2f}건",
        f"{closest_to_mean_district} ({district_tows[closest_to_mean_district]}건)",
        f"{median_tows:.2f}건",
        f"{closest_to_median_district} ({district_tows[closest_to_median_district]}건)"
    ]
}
summary_df = pd.DataFrame(summary_data)

# 두 DataFrame을 옆에 나란히 표시
html_output = f"""
<div style="display: flex; justify-content: space-around;">
    <div style="flex: 1; margin-right: 10px;">
        <h3>구별 견인 건수 총합 표</h3>
        {district_tows_df.to_html(index=False)}
    </div>
    <div style="flex: 1; margin-left: 10px;">
        <h3>견인 건수 총합 요약 통계</h3>
        {summary_df.to_html(index=False)}
    </div>
</div>
"""
display(HTML(html_output))

if '유형' in df.columns:
    # '미확인' 값을 제외하고 유형별 빈도 분석
    type_frequency_filtered = df[df['유형'] != '미확인']['유형'].value_counts()
    print("\n'미확인'을 제외한 유형별 빈도:")
    display(type_frequency_filtered)
else:
    print("'유형' 컬럼이 없어 유형별 빈도 분석을 수행할 수 없습니다.")

import matplotlib.pyplot as plt
import matplotlib.font_manager as fm

# 나눔 고딕 폰트 설치 (설치되어 있지 않은 경우)
!apt-get update -qq
!apt-get install fonts-nanum* -qq

# 폰트 경로 설정
font_path = '/usr/share/fonts/truetype/nanum/NanumGothic.ttf'
fm.fontManager.addfont(font_path)

# 폰트 캐시 재생성 및 폰트 설정
plt.rcParams['font.family'] = 'NanumGothic'
plt.rcParams['axes.unicode_minus'] = False # 마이너스 기호 깨짐 방지

print("한글 폰트 설정 완료")

# 구별 견인 건수 총합을 내림차순으로 정렬
district_tows_sorted = district_tows.sort_values(ascending=False)

# 견인 건수가 0인 구는 제외
district_tows_filtered = district_tows_sorted[district_tows_sorted > 0]

# 견인 건수가 100건 미만인 구는 추가로 제외
district_tows_filtered = district_tows_filtered[district_tows_filtered >= 100]

# 막대 그래프 그리기
plt.figure(figsize=(15, 8))
plt.bar(district_tows_filtered.index, district_tows_filtered.values, color='steelblue', edgecolor='black')

plt.title('구별 전동킥보드 견인 건수 총합', fontsize=16)
plt.xlabel('구', fontsize=12)
plt.ylabel('견인 건수 총합', fontsize=12)
plt.xticks(rotation=45, ha='right', fontsize=10) # X축 레이블 회전 및 정렬
plt.yticks(fontsize=10)
plt.grid(axis='y', alpha=0.75, linestyle='--') # Y축에 그리드 추가
plt.tight_layout() # 그래프 요소들이 겹치지 않도록 자동 조정
plt.show()

import pandas as pd
from IPython.display import display, HTML

file_path_parking = '서울시 전동킥보드 주차구역 현황.csv'

try:
    df_parking = pd.read_csv(file_path_parking, encoding='cp949', engine='python')
    print(f"'{file_path_parking}' 파일 로드 성공!")
except FileNotFoundError:
    print(f"오류: '{file_path_parking}' 파일을 찾을 수 없습니다. 파일을 Colab 환경에 업로드해주세요.")
    # Colab에서 파일을 업로드하려면 다음 코드를 사용하세요:
    # from google.colab import files
    # uploaded = files.upload()
    # for fn in uploaded.keys():
    #   print(f'User uploaded file "{fn}" with length {len(uploaded[fn])} bytes')
except pd.errors.ParserError as e:
    print(f"오류: '{file_path_parking}' 파일을 파싱하는 데 문제가 발생했습니다: {e}")
    print("엔진을 'python'으로 변경하여 재시도합니다.")
    try:
        df_parking = pd.read_csv(file_path_parking, encoding='cp949', engine='python')
    except Exception as e_inner:
        print(f"재시도 실패: {e_inner}")
except Exception as e:
    print(f"파일 로드 중 알 수 없는 오류 발생: {e}")

# '구정보' 컬럼이 있는지 확인하고 없으면 다른 컬럼을 찾아볼 수 있도록 안내
if '구정보' not in df_parking.columns:
    print("'구정보' 컬럼을 찾을 수 없습니다. '시군구명' 컬럼을 사용합니다.")
    # print("현재 데이터프레임의 컬럼: ", df_parking.columns.tolist())
    district_column = '시군구명'
else:
    district_column = '구정보'


# '시군구명'으로 주차구역 수의 총합을 구하기
# 각 행이 하나의 주차구역을 의미한다고 가정하고 .size()를 사용
district_parking_counts = df_parking.groupby(district_column).size()
district_parking_counts.name = '주차구역 수 총합' # 결과 Series에 이름 부여

# 총합이 최소인 구, 최대인 구 찾기
min_parking_district = district_parking_counts.idxmin()
max_parking_district = district_parking_counts.idxmax()

# 평균값과 중간값 계산
mean_parking_counts = district_parking_counts.mean()
median_parking_counts = district_parking_counts.median()

# 평균값에 가장 가까운 구 찾기
closest_to_mean_parking = (district_parking_counts - mean_parking_counts).abs().idxmin()

# 중간값에 가장 가까운 구 찾기
closest_to_median_parking = (district_parking_counts - median_parking_counts).abs().idxmin()

# --- 구별 주차구역 수 총합 표 생성 ---
district_parking_df = district_parking_counts.reset_index()
district_parking_df.columns = [district_column, '주차구역 수 총합']

# --- 요약 통계 DataFrame 생성 ---
summary_parking_data = {
    '지표': ['최소 주차구역 수 구', '최대 주차구역 수 구', '평균 주차구역 수', '평균값에 가장 가까운 구', '중간값 주차구역 수', '중간값에 가장 가까운 구'],
    '값': [
        f"{min_parking_district} ({district_parking_counts.min()}개)",
        f"{max_parking_district} ({district_parking_counts.max()}개)",
        f"{mean_parking_counts:.2f}개",
        f"{closest_to_mean_parking} ({district_parking_counts[closest_to_mean_parking]}개)",
        f"{median_parking_counts:.2f}개",
        f"{closest_to_median_parking} ({district_parking_counts[closest_to_median_parking]}개)"
    ]
}
summary_parking_df = pd.DataFrame(summary_parking_data)

# 두 DataFrame을 옆에 나란히 표시
html_output_parking = f"""
<div style="display: flex; justify-content: space-around;">
    <div style="flex: 1; margin-right: 10px;">
        <h3>구별 주차구역 수 총합 표</h3>
        {district_parking_df.to_html(index=False)}
    </div>
    <div style="flex: 1; margin-left: 10px;">
        <h3>주차구역 수 총합 요약 통계</h3>
        {summary_parking_df.to_html(index=False)}
    </div>
</div>
"""
display(HTML(html_output_parking))

# '시군구명' 컬럼이 있는지 확인
if '시군구명' in df_parking.columns:
    # '구 정보' 컬럼 생성: '시군구명' 컬럼을 그대로 사용
    df_parking['구 정보'] = df_parking['시군구명']
    # '주소' 컬럼 생성: 기존 '주소' 컬럼을 그대로 사용
    df_parking['주소'] = df_parking['주소'] # Assuming '주소' column already exists in the original data
else:
    print("'시군구명' 컬럼이 없습니다. '구 정보' 생성을 건너뜁니다.")
    df_parking['구 정보'] = '미확인'
    df_parking['주소'] = '미확인'

# '거치대 유무' 컬럼이 있는지 확인하여 '거치대 유무 정보' 컬럼 생성
if '거치대 유무' in df_parking.columns:
    # 'Y'는 '있음', 'N'은 '없음'으로 매핑
    df_parking['거치대 유무 정보'] = df_parking['거치대 유무'].map({'Y': '있음', 'N': '없음'})
else:
    print("'거치대 유무' 컬럼이 없습니다. '거치대 유무 정보'를 '정보 없음'으로 처리합니다.")
    df_parking['거치대 유무 정보'] = '정보 없음'

print("\n'구 정보', '주소', '거치대 유무 정보' 컬럼 생성 후 데이터프레임 상위 5개 행:")
display(df_parking.head())

if '구 정보' in df_parking.columns and '거치대 유무 정보' in df_parking.columns:
    # '구 정보'와 '거치대 유무 정보'를 기준으로 교차표(crosstab) 생성
    district_rack_analysis = pd.crosstab(df_parking['구 정보'], df_parking['거치대 유무 정보'])
    print("\n자치구별 거치대 유무에 따른 주차구역 수:")
    display(district_rack_analysis)
else:
    print("'구 정보' 또는 '거치대 유무 정보' 컬럼이 없어 분석을 수행할 수 없습니다.")

import matplotlib.pyplot as plt
import matplotlib.font_manager as fm

# 폰트 설정 (이전 셀에서 이미 설치 및 설정되었을 것으로 가정)
plt.rcParams['font.family'] = 'NanumGothic'
plt.rcParams['axes.unicode_minus'] = False # 마이너스 기호 깨짐 방지

# 주차구역 수가 10개 미만인 구는 제외
district_parking_filtered = district_parking_counts[district_parking_counts >= 10]

# 내림차순으로 정렬
district_parking_filtered = district_parking_filtered.sort_values(ascending=False)

# 막대 그래프 그리기
plt.figure(figsize=(15, 8))
plt.bar(district_parking_filtered.index, district_parking_filtered.values, color='skyblue', edgecolor='black')

plt.title('구별 전동킥보드 주차구역 수 총합 (10개 미만 제외)', fontsize=16)
plt.xlabel('구', fontsize=12)
plt.ylabel('주차구역 수 총합', fontsize=12)
plt.xticks(rotation=45, ha='right', fontsize=10) # X축 레이블 회전 및 정렬
plt.yticks(fontsize=10)
plt.grid(axis='y', alpha=0.75, linestyle='--') # Y축에 그리드 추가
plt.tight_layout() # 그래프 요소들이 겹치지 않도록 자동 조정
plt.show()

import pandas as pd

# district_tows와 district_parking_counts를 DataFrame으로 변환
# '구정보'와 '시군구명'이 동일한 의미의 구 이름을 가리킨다고 가정하고 병합
tows_df = district_tows.reset_index()
tows_df.columns = ['구정보', '견인건수'] # '견인건수 총합'을 '견인건수'로 변경

parking_df = district_parking_counts.reset_index()
parking_df.columns = ['구정보', '주차구역 수'] # '주차구역 수 총합'을 '주차구역 수'로 변경

# 두 데이터를 '구정보'를 기준으로 외부 조인(outer join)하여 모든 구를 포함
combined_data = pd.merge(tows_df, parking_df, on='구정보', how='outer')

# 견인건수 또는 주차구역 수가 없는 경우 0으로 채우기
combined_data = combined_data.fillna(0)

# '총합' 컬럼 계산: 견인건수와 주차구역 수를 더함
combined_data['총합'] = combined_data['견인건수'] + combined_data['주차구역 수']

# 종합 점수가 높은 순서대로 정렬하고 상위 5개 구 선택
final_top_5_combined_score = combined_data.sort_values(by='총합', ascending=False).head(5).reset_index(drop=True)

# 인덱스를 1부터 시작하도록 변경
final_top_5_combined_score.index = final_top_5_combined_score.index + 1

# 결과 출력 및 형식 지정
display(final_top_5_combined_score.style.format({
    '견인건수': "{:.0f}",
    '주차구역 수': "{:.0f}",
    '총합': "{:.0f}"
}))

# '총합' 점수가 가장 높은 상위 1개 구 선택
top_district_for_app = final_top_5_combined_score.iloc[[0]]

# 결과 출력 및 형식 지정
print("앱 개발을 위해 선정된 최적의 구:")
display(top_district_for_app.style.format({
    '견인건수': "{:.0f}",
    '주차구역 수': "{:.0f}",
    '총합': "{:.0f}"
}))

import matplotlib.pyplot as plt
import matplotlib.font_manager as fm
import pandas as pd
import os

nanum_font_name = 'NanumBarunGothic'
nanum_font_path_expected = '/usr/share/fonts/truetype/nanum/NanumBarunGothic.ttf'

# 1. 폰트 설치 여부 확인 및 설치
if not os.path.exists(nanum_font_path_expected):
    print("한글 폰트가 시스템에 없습니다. NanumBarunGothic 폰트를 설치합니다.")
    !sudo apt-get update -qq > /dev/null
    !sudo apt-get install -y fonts-nanum > /dev/null
    !sudo fc-cache -fv > /dev/null # 시스템 폰트 캐시 갱신
    !rm -rf ~/.cache/matplotlib # Matplotlib 캐시 삭제

    print("폰트 설치를 완료했습니다. **런타임을 다시 시작(Runtime -> Restart runtime)한 후 다시 실행해 주세요.**")
    # 런타임 재시작을 강력히 권고합니다. 이 시점에서 코드를 종료하여 재시작을 유도할 수도 있으나,
    # 에이전트는 사용자의 의도를 존중하고 다음 셀 실행을 막지 않습니다.
    # 사용자가 재시작을 하지 않았다면, 이 셀은 폰트가 없는 상태로 다음 단계를 진행하게 됩니다.

# 2. Matplotlib 폰트 캐시 재빌드 (런타임 재시작 후에도 필요할 수 있음)
# Matplotlib의 내부 폰트 캐시를 비우는 직접적인 함수가 존재하지 않거나 버전 문제로 오류가 발생할 수 있으므로
# 캐시 파일 삭제와 폰트 매니저 스캔에 의존합니다.
# 새로운 폰트 경로를 명시적으로 추가하여 Matplotlib이 인지하도록 합니다.
if os.path.exists(nanum_font_path_expected):
    fm.fontManager.addfont(nanum_font_path_expected)
# 폰트 매니저에게 폰트를 다시 스캔하도록 요청합니다.
# rebuild_if_missing=True는 Matplotlib의 폰트 캐시를 다시 빌드하는 역할을 합니다.
fm.findfont(nanum_font_name, rebuild_if_missing=True)


# 3. 사용할 폰트 설정
# 직접 폰트 파일 경로를 사용하여 FontProperties를 생성하는 것이 가장 확실합니다.
try:
    if os.path.exists(nanum_font_path_expected):
        font_prop = fm.FontProperties(fname=nanum_font_path_expected)
        plt.rcParams['font.family'] = font_prop.get_name()
        print(f"Matplotlib 폰트 설정 완료: {plt.rcParams['font.family']} (경로 지정)")
    else:
        # 경로로 찾지 못했으면, 시스템에 등록된 이름으로 찾아봅니다.
        # 이전에 설치를 시도했으므로, 재시작 후에는 성공할 가능성이 높습니다.
        if fm.findfont(nanum_font_name):
            plt.rcParams['font.family'] = nanum_font_name
            print(f"Matplotlib 폰트 설정 완료: {plt.rcParams['font.family']} (이름 지정)")
        else:
            raise ValueError(f"폰트 '{nanum_font_name}'를 찾을 수 없습니다.")

except Exception as e:
    print(f"경고: 한글 폰트 설정 중 오류 발생 - {e}. 기본 폰트(DejaVu Sans)로 대체합니다.")
    plt.rcParams['font.family'] = 'DejaVu Sans' # 최후의 보루
finally:
    plt.rcParams['axes.unicode_minus'] = False # 마이너스 기호 깨짐 방지

print(f"현재 설정된 Matplotlib 폰트: {plt.rcParams['font.family']}")

# 필요한 라이브러리 임포트
import requests
from google.colab import userdata
import folium
import time
import numpy as np

# Colab Secrets에서 Kakao Map API 키 로드
KAKAO_API_KEY = userdata.get('KAKAO_MAP_API_KEY')

if KAKAO_API_KEY is None:
    print("오류: 'KAKAO_MAP_API_KEY'가 Colab Secrets에 설정되지 않았습니다. API 키를 설정해주세요.")
else:
    print("카카오 API 키 로드 완료.")

def get_coordinates_kakao(address):
    """주소를 위도, 경도로 변환하는 함수 (Kakao Map API 사용)"""
    if KAKAO_API_KEY is None:
        return None, None

import pandas as pd
import folium
import time
from IPython.display import display

# 파일 경로 설정
file_path_parking = '/content/서울시 전동킥보드 주차구역 현황.csv'

# CSV 파일 로드 및 인코딩 처리
try:
    df_parking = pd.read_csv(file_path_parking, encoding='cp949')
except UnicodeDecodeError:
    print("cp949 인코딩으로 파일을 읽는 데 실패했습니다. euc-kr 인코딩을 시도합니다.")
    df_parking = pd.read_csv(file_path_parking, encoding='euc-kr')

# '시군구명' 컬럼이 있는지 확인
if '시군구명' in df_parking.columns:
    df_parking['구 정보'] = df_parking['시군구명']
else:
    df_parking['구 정보'] = '미확인'

# '거치대 유무' 컬럼이 있는지 확인하여 '거치대 유무 정보' 컬럼 생성
if '거치대 유무' in df_parking.columns:
    df_parking['거치대 유무 정보'] = df_parking['거치대 유무'].map({'Y': '있음', 'N': '없음'})
else:
    df_parking['거치대 유무 정보'] = '정보 없음'

# '미확인' 주소 제거
parking_data_filtered = df_parking[df_parking['주소'] != '미확인'].copy()

print(f"주차 데이터 개수: {len(parking_data_filtered)}개")

# Naver Maps API는 전체 주소 형식을 선호할 수 있으므로, '서울특별시'를 주소 앞에 추가
parking_data_filtered['full_address'] = parking_data_filtered['주소'].apply(lambda x: f"서울특별시 {x}" if not x.startswith('서울특별시') else x)

# 위도, 경도 컬럼 초기화
parking_data_filtered['latitude'] = None
parking_data_filtered['longitude'] = None

print("\n주소 지오코딩을 시작합니다. 데이터 양에 따라 시간이 다소 소요될 수 있습니다...")
# 주소 지오코딩
for idx, row in parking_data_filtered.iterrows():
    if (idx + 1) % 50 == 0: # 50개마다 진행 상황 출력
        print(f"{idx + 1}/{len(parking_data_filtered)} 주소 처리 중...")
    lat, lon = get_coordinates_naver(row['full_address']) # Naver Geocoding API 사용
    parking_data_filtered.at[idx, 'latitude'] = lat
    parking_data_filtered.at[idx, 'longitude'] = lon
    time.sleep(0.1) # API 호출 제한을 피하기 위해 지연 시간을 늘립니다.

# 유효한 좌표만 남김
parking_data_geo = parking_data_filtered.dropna(subset=['latitude', 'longitude'])

print("\n주차 구역 지오코딩 결과:")
display(parking_data_geo.head())

# Folium 맵 생성 (서울 중심 좌표)
m_parking = folium.Map(location=[37.5665, 126.9780], zoom_start=12)

# 각 주차 구역 위치를 마커로 추가
for idx, row in parking_data_geo.iterrows():
    folium.Marker(
        location=[row['latitude'], row['longitude']],
        popup=f"<b>구:</b> {row['구 정보']}<br><b>주소:</b> {row['주소']}<br><b>거치대 유무:</b> {row['거치대 유무 정보']}",
        icon=folium.Icon(color='blue', icon='info-sign')
    ).add_to(m_parking)

# 맵을 HTML 파일로 저장
parking_map_path = 'seoul_parking_map_naver.html'
m_parking.save(parking_map_path)

print(f"\n주차 구역 지도가 '{parking_map_path}' 파일로 저장되었습니다. 파일을 열어 확인해 주세요.")
# Colab에서 HTML 파일을 바로 표시하려면:
display(m_parking)

