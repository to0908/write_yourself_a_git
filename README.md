# write_yourself_a_git
https://wyag.thb.lt/#getting-started

gitの動作確認
```
mkdir test
cd test
git init
tree .git

echo "hello" > a.txt
git add a.txt
git commit -m "first commit"
tree .git
```

wyag init
```
# current dirがwrite_yourself_a_gitであることを確認する
# cd ..
wyag init test2
cd test2
tree .git

echo "hello" > a.txt
git add a.txt
git commit -m "first"
tree .git
```

Git Object
```
find .git/objects/ -type f
xxd .git/objects/~/~~
# 実際に見てみるとバイナリファイルで、これを見てもよくわかりません
```

wyagの準備
```
cp ../libwyag.py libwyag.py
cp ../wyag.py wyag.py
echo -e "*wyag.py\n__pycache__" > .gitignore
```

cat-file, 動作確認
```
find .git/objects/ -type f
# 1つずつオブジェクトを確認していく
# blobだけ中身を確認

git cat-file -t (OBJECT ID)
git cat-file blob (OBJECT ID)
git cat-file -p (OBJECT ID)

wyag cat-file blob (OBJECT ID)

wyag cat-file blob (OBJECT ID) > a.txt
git cat-file blob (OBJECT ID) | diff - a.txt -s
```


hash-object, blobデータで動作確認
```
git hash-object wyag
wyag hash-object wyag
```


HEADの位置のhash
```
git rev-parse HEAD
git cat-file -p (HEAD ID)
```

commit parse
現時点で1回しかcommitしていないため、parentが存在しない
```
git cat-file commit (COMMIT ID)
wyag cat-file commit (COMMIT ID)

wyag cat-file commit (COMMIT ID) > a.txt
git cat-file commit (COMMIT ID) | diff - a.txt -s
```

もう一度commitしてみる.
実はさっき.gitignoreを作っているのでもうcommitしてもよいが、
今度はsrc/下にb.pyを作る
```
mkdir src
echo "print(1)" > src/b.py
python3 src/b.py
git add .
git commit -m "b.py"
```

これで直接HEAD(commit id)のtreeを見れる
```
git cat-file -p HEAD^{tree}
```

ls-tree
```
git ls-tree HEAD^{tree}
git ls-tree HEAD
wyag ls-tree HEAD
```


checkout
```
tree
git rev-parse HEAD
echo "c" > c.txt
git add .
git commit -m "c.txt"
git rev-parse HEAD 

# HEADのIDが変わっていることを確認した

mkdir dir
wyag checkout (前のCOMMIT ID) dir
tree dir
```