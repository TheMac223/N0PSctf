![image](https://github.com/DA2RIM/N0PSctf/assets/171825457/4134a13b-bf82-44cd-9ca6-f826c57e1df7)문제 파일은 img 확장자인 파일인데 바이너리를 보면 
![image](https://github.com/DA2RIM/N0PSctf/assets/171825457/08654619-2ea0-4480-aaff-a3665e2cd6fc)

바이너리를 뒤집어야 할거같은 구조
```python
def reverse_binary_file(input_file, output_file):
    with open(input_file, 'rb') as f:
        data = f.read()
    
    reversed_data = data[::-1]
    
    with open(output_file, 'wb') as f:
        f.write(reversed_data)

# 입력 파일과 출력 파일 경로를 지정합니다.
input_file = './Reverse Me/img.jpg'
output_file = 'output.bin'

# 함수 호출
reverse_binary_file(input_file, output_file)
```

뒤집고나면 역시나 ELF 파일 형태가 보이는데 

여기서부터 해결을 못해서 flag를 얻는데는 실패했다,,


IDA 로 확인하면 main이 이런구조인데
![image](https://github.com/DA2RIM/N0PSctf/assets/171825457/da2123f1-bfa6-40e8-91ff-9bc9671565a9)

간단하게 봤을 때
```python
if ( sub_1460(v3, v4, v5, v6) )
    {
      v7 = -v6;
      if ( v6 > 0 )                             // v6 가 0보다 큼
        v7 = v6;
      v12 = v7;
      v8 = -v5;
      if ( v5 > 0 )
        v8 = v5;
      v9 = -v4;
      if ( v4 > 0 )
        v9 = v4;
      v10 = -v3;
      if ( v3 > 0 )
        v10 = v3;
      __sprintf_chk(v15, 1LL, 42LL, "%d%d%d%d", v10, v9, v8, v12);
      qmemcpy(src, &unk_2016, 0x19uLL);
      v11 = sub_1A50(src, 0x18uLL, v15, v13);
      puts(v11);
      free(v11);
      exit(0);
    }
```
첫 if 문에 들어가지 못한다면 exit(-1) 으로 프로그램이 비정상적 종료가된다.

즉 sub_1460 함수만 확인하면 어떤 매개변수를 넣어야할지 알수있다는건데 나는 그걸모르고 sub_1460 과 sub_1A50 을 같이 신경썼고 문제풀이에 실패했다.

롸업을 쓴 사람꺼를 보니깐 일단 어떤 매개변수인지 z3로 알아낸다음에 실제로 프로그램을 실행시키니 플래그가 떴다.

```python
from z3 import *

part1 = BitVec('part1', 32)
part2 = BitVec('part2', 32)
part3 = BitVec('part3', 32)
part4 = BitVec('part4', 32)

solver = Solver()

solver.add(part1 * -10 + part2 * 4 + part3 + part4 * 3 == 0x1c)
solver.add(part2 * 9 + part1 * -8 + part3 * 6 + part4 * -2 == 0x48)
solver.add(part2 * -3 + part1 * -2 + part3 * -8 + part4 == 0x1d)
solver.add(part2 * 7 + part1 * 5 + part3 + part4 * -6 == 0x58)

if solver.check() == sat:
    model = solver.model()
    print(f"part1: {model[part1].as_long()}")
    print(f"part2: {model[part2].as_long()}")
    print(f"part3: {model[part3].as_long()}")
    print(f"part4: {model[part4].as_long()}")
else:
    print("No solution found")
```

