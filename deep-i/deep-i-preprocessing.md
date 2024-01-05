# deep-i-reprocessing

- 딥아이 근로중 개발한 코드들을 저장

## Deep-i Preprocessing

```python
import os

def deepi_preprocessing(dir_path:str) -> None:
    '''
    데이터: class, 중점 x, 중점 y, 너비, 높이
    폴더열고, 용량 0인 txt 파일과 같은 이름은 png 삭제
    dir_path: 폴더 경로
    dir_list: 폴더 내 파일 리스트
    file_size: 폴더 내 파일 개수
    remove_list: 삭제할 파일 리스트
    '''
    
    dir_list = os.listdir(dir_path)
    file_size = len(dir_list)
    
    print(f"Loading {file_size} files.")
    
    remove_list = []
    
    for i in dir_list:
        file_name, ext = i.split(".")
        
        if ext == "txt" and os.path.getsize(dir_path + i) == 0:
            remove_list.append(file_name)
    
    print(f"Check {len(remove_list)} files.")
    
    try:
        for i in remove_list:
            os.remove(dir_path + i + ".png")
            os.remove(dir_path + i + ".txt")
    except:
        print("Error: Can't remove files.")
        
    print(f"Remove {file_size - len(os.listdir(dir_path))} files.")
    
    print("Done.")

if __name__ == "__main__":
    deepi_preprocessing("./origin_train_data/")

```

- 데이터: class, 중점 x, 중점 y, 너비, 높이로 되어있는 txt 파일과 이미지가 같은 파일명을 가지고 있어야 함.
