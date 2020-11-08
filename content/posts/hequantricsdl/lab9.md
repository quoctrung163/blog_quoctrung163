+++
title = "Lab 9&Lab10"
date = "2020-11-07"
author = "quoctrung163"
authorTwitter = "" #do not include @
cover = ""
tags = ["", "hqtcsdl"]
keywords = ["", ""]
description = ""
showFullContent = false
+++

### Tạo 10 mẫu tin (nhập dữ liệu) cho 10 sinh viên. Sau đó thực hiện các thao tác CRUD (Create, Read, Update, Delete) trên dữ liệu mẫu. Được tuỳ chọn ngôn ngữ lập trình

### 1. Mongodb
- Để bắt đầu sử dụng mongodb trên Node.js, hãy cài đặt module mongodb
```cmd
mkdir crudsinhvien
cd crudsinhvien
yarn init
yarn add mongodb
```

- Chạy mongo trên docker => cổng 27015
{{< figure src="/img/heqtcsdl/lab9-10/1.png" title="lab9" >}}

- Import, Require Module
```ts
const { MongoClient } = require('mongodb');
const databaseName = 'crudsinhvien';
const config = { useNewUrlParser: true, useUnifiedTopology: true }
let url = 'mongodb://localhost:27015/';
```
{{< linebreak >}}
- C (Create)
- Tạo database crudsinhvien và chèn dữ liệu 10 sinh viên
```js
MongoClient.connect(url, config, (err, db) => {
  if (err) throw err;
  let dbo = db.db(`${databaseName}`);
  let svArr = [{
    MSSV: '1710289', Hoten: 'Phan Quốc Trung', Namsinh: 1999, DiemMon1: 8, DiemMon2: 7, DiemMon3: 10, Email: '1710289@dlu.edu.vn', DienThoai: '0349981228'
  }, {
    MSSV: '1710233', Hoten: 'Đặng Trần Hữu Nhân', Namsinh: 1999, DiemMon1: 8, DiemMon2: 7, DiemMon3: 10, Email: '1710233@dlu.edu.vn', DienThoai: '0349981228'
  }, {
    MSSV: '1710196', Hoten: 'Nguyễn Đăng Khoa', Namsinh: 1999, DiemMon1: 8, DiemMon2: 7, DiemMon3: 10, Email: '1710196@dlu.edu.vn', DienThoai: '0349981228'
  }, {
    MSSV: '1714234', Hoten: 'Hứa Đình Doanh', Namsinh: 1999, DiemMon1: 8, DiemMon2: 7, DiemMon3: 10, Email: '1714234@dlu.edu.vn', DienThoai: '0349981228'
  }, {
    MSSV: '1710264', Hoten: 'Huỳnh Lê Hữu Thành', Namsinh: 1999, DiemMon1: 8, DiemMon2: 7, DiemMon3: 10, Email: '1710264@dlu.edu.vn', DienThoai: '0349981228'
  }, {
    MSSV: '1710144', Hoten: 'Nguyễn Đức Đề', Namsinh: 1999, DiemMon1: 8, DiemMon2: 7, DiemMon3: 10, Email: '1710144@dlu.edu.vn', DienThoai: '0349981228'
  }, {
    MSSV: '1710156', Hoten: 'Phạm Bá Xuân Duy', Namsinh: 1999, DiemMon1: 8, DiemMon2: 7, DiemMon3: 10, Email: '1710156@dlu.edu.vn', DienThoai: '0349981228'
  }, {
    MSSV: '1710204', Hoten: 'Bùi Đức Hoàng Lâm', Namsinh: 1999, DiemMon1: 8, DiemMon2: 7, DiemMon3: 10, Email: '1710204@dlu.edu.vn', DienThoai: '0349981228'
  }, {
    MSSV: '1710303', Hoten: 'Phạm Hoàng Việt', Namsinh: 1999, DiemMon1: 8, DiemMon2: 7, DiemMon3: 10, Email: '1710303@dlu.edu.vn', DienThoai: '0349981228'
  }, {
    MSSV: '1710285', Hoten: 'Lê Anh Trí', Namsinh: 1999, DiemMon1: 8, DiemMon2: 7, DiemMon3: 10, Email: '1710289@dlu.edu.vn', DienThoai: '0349981228'
  }];

  dbo.collection('sinhvien').insertMany(svArr, (err, res) => {
    if (err) throw err;
    console.log('Đã chèn dữ liệu sinh viên thành công!');
    db.close();
  });
})
```
{{< figure src="/img/heqtcsdl/lab9-10/3.png" title="lab9" >}}

{{< linebreak >}}
- R (Read): đọc dữ liệu tất cả sinh viên
```js
MongoClient.connect(url, config, (err, db) => {
  if (err) throw err;
  const dbo = db.db(`${databaseName}`);
  dbo.collection('sinhvien').find({}).toArray(function (err, result) {
    if (err) throw err;
    console.log(result);
    db.close();
  });
})
```
{{< figure src="/img/heqtcsdl/lab9-10/4.png" title="lab9" >}}

{{< linebreak >}}
- D (Delete): Xoá một sinh viên nào đó (vd: sv có mssv: 1710289)

```js
let MSSV = '1710289';

MongoClient.connect(url, config, (err, db) => {
  if (err) throw err;
  const dbo = db.db(`${databaseName}`);
  let removeQuery = { MSSV };
  dbo.collection('sinhvien').deleteOne(removeQuery, (err, obj) => {
    if (err) throw err;
    console.log(`sinh viên có mã ${MSSV} đã bị xoá`);
    db.close();
  })
})
```
{{< figure src="/img/heqtcsdl/lab9-10/5.png" title="lab9" >}}

{{< linebreak >}}
- U (Update): thay đổi điểm sinh viên bất kì (vd sv có mã 1710285)

```ts
let MSSV = '1710285';

MongoClient.connect(url, config, (err, db) => {
  if (err) throw err;
  const dbo = db.db(`${databaseName}`);
  let updateQuery = { MSSV };
  let newScore = {
    $set: { DiemMon1: 10, DiemMon2: 10, DiemMon3: 10 }
  };
  dbo.collection('sinhvien').updateOne(updateQuery, newScore, (err, res) => {
    if (err) throw err;
    console.log(`sinh viên ${MSSV} đã được cập nhật`);
    db.close();
  });
  dbo.collection('sinhvien').find({ MSSV }).toArray(function (err, result) {
    if (err) throw err;
    console.log(result);
    db.close();
  });
});
```
{{< figure src="/img/heqtcsdl/lab9-10/6.png" title="lab9" >}}

{{< linebreak >}}
### 2. Mysql
- Install module mysql2
```json
yarn init
yarn add mysql2
```

- Khởi động mysql docker cổng 3305
{{< figure src="/img/heqtcsdl/lab9-10/8.png" title="lab9" >}}

- Import 
```ts
const mysql = require('mysql2');

const connection = mysql.createConnection({
  host: 'localhost',
  port: 3305,
  user: 'root',
  database: 'qlsv',
  password: '12345678',
  multipleStatements: true
});
```

{{< linebreak >}}
- C (Create)
- Tạo database crudsinhvien và chèn dữ liệu 10 sinh viên
```ts
const createTableQuery = `
  create table SinhVien (
    MSSV char(8) primary key,
    Hoten varchar(30) not null,
    Namsinh int,
    DiemMon1 float,
    DiemMon2 float,
    DiemMon3 float,
    Email char(30),
    Dienthoai char(11)
  );
`;

const insertquery = `
  insert into SinhVien values ('1710289', 'Phan Quốc Trung', 1999, 8, 9, 9, '1710289@dlu.edu.vn', '0349981228');
  insert into SinhVien values ('1710233', 'Đặng Trần Hữu Nhân', 1999, 5, 5, 5, '1710233@dlu.edu.vn', '035547878');
  insert into SinhVien values ('1710196', 'Nguyễn Đăng Khoa', 1999, 7, 9, 9, '1710196@dlu.edu.vn', '035547878');
  insert into SinhVien values ('1714234', 'Hứa Đình Doanh', 1999, 7, 9, 9, '1714234@dlu.edu.vn', '035547878');
  insert into SinhVien values ('1710264', 'Huỳnh Lê Hữu Thành', 1999, 8, 9, 9, '1710264@dlu.edu.vn', '035547878');
  insert into SinhVien values ('1710144', 'Nguyễn Đức Đề', 1999, 8, 9, 9, '1710144@dlu.edu.vn', '035547878');
  insert into SinhVien values ('1710156', 'Phạm Bá Xuân Duy', 1999, 8, 9, 9, '1710156@dlu.edu.vn', '035547878');
  insert into SinhVien values ('1710204', 'Bùi Đức Hoàng Lâm', 1999, 8, 9, 9, '1710204@dlu.edu.vn', '035547878');
  insert into SinhVien values ('1710303', 'Phạm Hoàng Việt', 1999, 8, 9, 9, '1710303@dlu.edu.vn', '035547878');
  insert into SinhVien values ('1710285', 'Lê Anh Trí', 1999, 8, 9, 9, '1710285@dlu.edu.vn', '035547878');
`;

connection.query(
  createTableQuery + insertquery,
  function (err, results, fields) {
    if (err === null) {
      console.log('đã chèn dữ liệu sinh viên thành công');
    }
  }
);
```
{{< figure src="/img/heqtcsdl/lab9-10/7.png" title="lab9" >}}
{{< linebreak >}}
- R (Read)
- Hiển thị tất cả sinh viên

```ts
const displayQuery = `
  select * from SinhVien;
`;
connection.query(
  displayQuery,
  function (err, results, fields) {
    if (err) throw err;
    console.log(results);
  }
);
```
{{< figure src="/img/heqtcsdl/lab9-10/9.png" title="lab9" >}}
{{< linebreak >}}

- U (Update)
- Cập nhật điểm sinh viên nào đó
```ts
const updateQuery = `
  update SinhVien
  set DiemMon1=10, DiemMon2=10, DiemMon3=10
  where MSSV='1710289';
`;

const displayQuery = `select * from SinhVien where MSSV=1710289`;

connection.query(
  updateQuery + displayQuery,
  function (err, results, fields) {
    if (err) throw err;
    console.log(results);
  }
);
```
{{< figure src="/img/heqtcsdl/lab9-10/10.png" title="lab9" >}}

{{< linebreak >}}
- D (Delete)
- Xoá sinh viên nào đó

```ts
const MSSV = '1710233';

const removeStudentQuery = `delete from SinhVien where MSSV=${MSSV};`

const displayQuery = `select * from SinhVien where MSSV=${MSSV}`;
connection.query(
  removeStudentQuery + displayQuery,
  function (err, results, fields) {
    if (err === null) {
      console.log(`Đã xoá sinh viên có mã ${MSSV} thành công!`);
    }
    console.log(results);
  }
);
```
{{< figure src="/img/heqtcsdl/lab9-10/11.png" title="lab9" >}}