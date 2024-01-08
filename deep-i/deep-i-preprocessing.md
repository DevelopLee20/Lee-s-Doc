# deep-i-reprocessing

- 딥아이 근로중 개발한 코드들을 저장

## Deep-i Preprocessing

```python
import os
import shutil
import random

def deepi_resetFolder(move_path):
    print(f"dir path: {move_path}")
    dir_move_list = os.listdir(move_path)
    print(f"Loading {len(dir_move_list)} files.")
    
    count = 0
    for i in dir_move_list:
        os.remove(os.path.join(move_path, i))
        count += 1
        
    print(f"Remove {count} files.\n")

def deepi_preprocessing(dir_path:str, move_path:str, rmdir:int=0) -> None:
    '''
    데이터: class, 중점 x, 중점 y, 너비, 높이
    V1, V2, V3-1 폴더 열어서 찾고, 전체 파일 all에 저장.
    그 후 랜덤 샘플링으로 train(8), test(2) 나누기.
    dir_path: 폴더 경로
    dir_list: 폴더 내 파일 리스트
    file_size: 폴더 내 파일 개수
    remove_list: 삭제할 파일 리스트
    '''
    
    if rmdir:
        deepi_resetFolder("./all")
    
    dir_list = os.listdir(dir_path)
    
    print(f"dir path: {dir_path}")
    print(f"Loading {len(dir_list)} files.")
    
    count = 0
    
    for i in dir_list:
        file_name, ext = i.split(".")
        
        if ext == "txt" and os.path.getsize(os.path.join(dir_path, i)) != 0:
            try:
                shutil.copy(os.path.join(dir_path, file_name + ".png"), os.path.join(move_path, file_name + ".png"))
                count += 1
                shutil.copy(os.path.join(dir_path, file_name + ".txt"), os.path.join(move_path, file_name + ".txt"))
                count += 1
            except:
                print(f"Error: '{file_name}' Can't move files.")
                exit()
    
    print(f"Move {count} files.")
    
    print(f"{dir_path} -> {move_path} Done.\n")

def deepi_validationSplit(dir_path:str, trainRatio:float, valDirName:list):
    '''
    all 폴더에 있는 전체를 체크 후 8, 2 비율로 나누어 train와 test 폴더에 안착인데, 폴더를 일단 지울까?
    '''
    if not (0.00 <= trainRatio <= 1.00):
        print("[Param error] trainRatio value is between 1.0 ~ 0.0")
    
    deepi_resetFolder(valDirName[0])
    deepi_resetFolder(valDirName[1])
    
    print(f"{valDirName} is empty.")
    
    dirlist = os.listdir(dir_path)
    
    all_count = int(len(dirlist) / 2)
    train_count = int(all_count * trainRatio)
    train_count = train_count - (train_count % 2)
    test_count = all_count - train_count
    
    print(f"find data: {all_count}")
    print(f"expect train data: {train_count}, expect test data: {test_count}")
    
    process_list = []
    
    while train_count:
        rand = random.randrange(0, len(dirlist))
        file_name, ext = dirlist.pop(rand).split(".")
        
        if file_name not in process_list:
            process_list.append(file_name)
            shutil.copy(os.path.join(dir_path, file_name + ".png"), os.path.join(valDirName[0], file_name + ".png"))
            shutil.copy(os.path.join(dir_path, file_name + ".txt"), os.path.join(valDirName[0], file_name + ".txt"))
            train_count -= 1
        
    while test_count:
        file_name, ext = dirlist.pop(0).split(".")
        
        if file_name not in process_list:
            process_list.append(file_name)
            shutil.copy(os.path.join(dir_path, file_name + ".png"), os.path.join(valDirName[1], file_name + ".png"))
            shutil.copy(os.path.join(dir_path, file_name + ".txt"), os.path.join(valDirName[1], file_name + ".txt"))
            test_count -= 1
        
    print(f"real train size: {int(len(os.listdir(valDirName[0])) / 2)}, real test size: {int(len(os.listdir(valDirName[1])) / 2)}")
        
    print("Done.\n")

if __name__ == "__main__":
    deepi_preprocessing("./V1", "./all", 1)
    deepi_preprocessing("./V2", "./all", 0)
    deepi_preprocessing("./V3-1", "./all", 0)
    deepi_validationSplit("./all", 0.80, ["./train", "./test"])


```
