# fuzzing_tiff

## Лабораторная работа 1
Проверка hash суммы
```
mkdir hashdir && cd hashdir
wget https://download.osgeo.org/libtiff/tiff-4.0.4.tar.gz
tar -xzvf tiff-4.0.4.tar.gz
find . type f -exec sha256sum {}  \; > hash.txt
cat hash.txt
```
![image](https://github.com/user-attachments/assets/e255dc02-2d75-4194-be35-1f81cd8eeac3)

## Лабораторная работа 2
Фаззинг тестирование проекта
```
wget https://download.osgeo.org/libtiff/tiff-4.0.4.tar.gz
tar -xzvf tiff-4.0.4.tar.gz
cd tiff-4.0.4/
CC=afl-clang-lto CFLAGS="--coverage" LDFLAGS="--coverage" ./configure --prefix="$HOME/fuzzing_tiff/install/" --disable-shared
AFL_USE_ASAN=1 make -j4
AFL_USE_ASAN=1 make install
afl-fuzz -m none -i $HOME/fuzzing_tiff/tiff-4.0.4/test/images/ -o $HOME/fuzzing_tiff/out/ -s 123 -- ./tools/tiffinfo -D -j -c -r -s -w @@
```
![image_2024-12-07_13-54-10](https://github.com/user-attachments/assets/9aef042b-320c-420f-a4c7-079192eeb17b)
```
afl-plot  $HOME/fuzzing_tiff/out/default/ plot_data
```
![image_2024-12-07_14-02-39](https://github.com/user-attachments/assets/7f6332d8-9560-42bc-9f27-0798bb5ce6ad)


## Лабораторная работа 3
Проверка покрытия
```
make clean
sudo apt install lcov
CC="gcc --coverage" CXX="g++ --coverage" ./configure
make
for file in /home/sovest/fuzzing_tiff/out_3/default/queue/*; do timeout 5 ./tools/tiffinfo -D -j -c -r -s -w "$file"  echo "Skipping $file"; done
lcov -o cov.info -c -d .
```
![image_2024-12-10_13-30-11](https://github.com/user-attachments/assets/fe7eb150-6886-4c7e-9ac2-0f7d3a308ac7)
```
 genhtml -o /home/sovest/shit/tiff-4.0.4 cov.info
```
![image_2024-12-10_13-29-55](https://github.com/user-attachments/assets/a60b62db-03ea-4e7d-aefc-a607b8b8f4d9)
![image_2024-12-10_13-29-27](https://github.com/user-attachments/assets/b2dc1484-a846-49e5-a3e4-8ebec2226181)

