import os
readme_content = """
# 전동 킥보드 안전 주차 및 견인 위험 지도

이 프로젝트는 서울시 전동 킥보드 주차 구역 현황과 강남구 견인 현황 데이터를 분석하여,
사용자가 견인 위험이 낮은 최적의 주차 공간을 찾을 수 있도록 돕는 인터랙티브 지도 웹페이지입니다.

## 주요 기능
- **주차 구역 마커**: 거치대 유무에 따른 주차 위치 표시
- **견인 핫스팟**: 견인이 자주 발생하는 구역을 히트맵으로 시각화

## 데이터 출처
- 서울시 열린데이터 광장
"""

with open('README.md', 'w', encoding='utf-8') as f:
    f.write(readme_content)

# 2. 업로드할 파일 리스트 확인
files_to_upload = ['index.html', 'parking_data_geo.csv', 'gangnam_towing_geo.csv', 'README.md']

print("--- 깃허브 저장소에 업로드할 파일 목록 ---")
for file in files_to_upload:
    if os.path.exists(file):
        print(f"✅ 준비됨: {file}")
    else:
        print(f"❌ 누락됨: {file} (이전 단계 코드를 다시 실행해주세요)")

print("\n위 파일들을 다운로드하여 깃허브 저장소(main 브랜치)에 업로드하신 후,")
print("Settings > Pages 메뉴에서 Build and deployment의 Branch를 'main'으로 설정하면 웹페이지가 배포됩니다.")
