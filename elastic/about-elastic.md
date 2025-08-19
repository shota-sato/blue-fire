# 第46回Elasticsearch勉強会 - Feb 16, 2022 - Elastic Meetup

![image](https://github.com/user-attachments/assets/c8e41943-554d-4d9f-8f8e-9e22eed28137)


![image](https://github.com/user-attachments/assets/6f55353e-c307-4ef1-8f08-50db288da6aa)


![image](https://github.com/user-attachments/assets/a644e6c5-61a5-4082-ae4f-0972a04e1e35)


![image](https://github.com/user-attachments/assets/7528c508-c4a6-4585-b429-bcf6a4bbe4bf)


![image](https://github.com/user-attachments/assets/1fc4f659-9ef1-4b79-aee7-4b54c5c1265f)


![image](https://github.com/user-attachments/assets/99e00499-699f-4a03-9231-d34e67ef3f5c)


![image](https://github.com/user-attachments/assets/e8f9405c-2f1d-44c9-b3fc-94fb43961e58)


## Ingest Pipeline

```
{
  "dot_expander": {
    "field": "*",
    "ignore_failure": true
  }
},
```
「ドットを含むフィールド名」を オブジェクトとして展開する役割を持ちます  

例）入力ドキュメント
```
{
  "user.name": "Shota",
  "user.age": 30
}
```
dot_expander 適用後
```
{
  "user": {
    "name": "Shota",
    "age": 30
  }
}
```
