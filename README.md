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

