+++
title = "Lab 10"
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
- Khởi tạo dự án, cài các module sau: express, mongodb, mongoose, body-parser,
method-override, validation, joi
```ts
mkdir crudsinhvien
cd crudsinhvien
yarn init
yarn add express mongodb mongoose body-parser
method-override validation joi
```

Trong đó:
- - express là framework của nodejs nó có các hàm để khởi tạo 1 web server dễ dàng.
- - mongodb là module cho hệ quản trị csdl mongodb
- - mongoose là module để định nghĩa collection và các kiểu dữ liệu field của 1 collection trong mongodb.
- - body-parser: là module để cung cấp các hàm tiện ích, parse được dữ liệu người dùng post lên như json, urlencoded, ..
- - method-override: nó là 1 module hỗ trợ các phương thức put, patch, delete dễ dàng
- - validation, joi: là các module validation

{{< linebreak >}}
Khởi tạo dự án import các module sử dụng trong chương trình
```ts
const express = require('express');
const app = express();

const mongoose = require('mongoose');
const bodyParser = require(`body-parser`);
const methodOverride = require('method-override');

const path = require(`path`);
```
{{< linebreak >}}
Thiết lập cơ bản
```ts
const databaseName = 'crudsinhvien';
const home = require('./routes/home');

// set template engine to using
app.set(`views`, `./views`);
app.set(`view engine`, `pug`);

// use method override
app.use(methodOverride('_method'));

// use static file 
app.use(express.static(path.join(__dirname, `public`)));

// use body-parser
app.use(bodyParser.json());
app.use(
  bodyParser.urlencoded({
    extended: true
  })
);

app.use('/', home);
```

{{< linebreak >}}
- Chạy mongo trên docker => cổng 27015
{{< figure src="/img/heqtcsdl/lab9-10/1.png" title="lab9" >}}
{{< linebreak >}}
- Kết nối mongodb trên docker
```ts
mongoose.connect(`mongodb://localhost:27015/${databaseName}`,
  { useUnifiedTopology: true },
  { useNewUrlParser: true },
  { useFindAndModify: false }
)
  .then(() => console.log('Connected to Mongodb...'))
  .catch(err => console.error('Could not connect to MongoDB', err));
```
{{< linebreak >}}
- Định nghĩa các trường dữ liệu trong collection sinh viên
```ts
const Joi = require('joi');
const mongoose = require('mongoose');

const SinhVien = mongoose.model('SinhVien',
  new mongoose.Schema({
    MSSV: {
      type: String,
      minlength: 6,
      default: "",
      maxlength: 8
    },
    Hoten: {
      type: String,
      minlength: 0,
      default: "",
      maxlength: 50
    },
    Namsinh: {
      type: Number,
      default: 0,
      minlength: 1,
      maxlength: 10
    },
    DiemMon1: {
      type: Number,
      default: 0,
      minlength: 1,
      maxlength: 2
    },
    DiemMon2: {
      type: Number,
      default: 0,
      minlength: 1,
      maxlength: 2
    },
    DiemMon3: {
      type: Number,
      default: 0,
      minlength: 1,
      maxlength: 2
    },
    Email: {
      type: String,
      default: "",
      minlength: 0,
      maxlength: 30
    },
    DienThoai: {
      type: String,
      default: "",
      minlength: 0,
      maxlength: 30
    }
  }), 'sinhvien');

```

{{< linebreak >}}
- Validation collection sinh viên
```ts
function validateSinhVien(sinhvien) {

  const schema = Joi.object({
    MSSV: Joi.string().min(6).max(8),
    Hoten: Joi.string().min(0).max(50),
    Namsinh: Joi.number(),
    DiemMon1: Joi.number(),
    DiemMon2: Joi.number(),
    DiemMon3: Joi.number(),
    Email: Joi.string().min(0).max(30),
    DienThoai: Joi.string().min(0).max(30)
  });

  return schema.validate(sinhvien);
}
```
{{< linebreak >}}
- Cuối cùng exports
```ts
module.exports = {
  SinhVien,
  validateSinhVien
}
```

- Lấy dữ liệu bên model và import vào routes/home.js và cài đặt các module cần thiết
- có thể hiểu nó là controller trong mô hình mvc
```ts
const { SinhVien, validateSinhVien } = require('../models/sinhvien');
const express = require('express');
const router = express.Router();
const validator = require('validator');
```
{{< linebreak >}}
- R (Read): hiển thị danh sách sinh viên
```ts
router.get('/', async (req, res, next) => {
  const sinhvien = await SinhVien.find();
  res.render('index', {
    sinhvien: sinhvien
  });
  next();
});
```
{{< linebreak >}}
- C (Create): thêm sinh viên
```ts
router.post('/', async (req, res, next) => {
  const { error } = await validateSinhVien(req.body);
  if (error) return res.status(400).send(error.details[0].message);

  let sinhvien = new SinhVien({
    MSSV: req.body.MSSV,
    Hoten: req.body.Hoten,
    Namsinh: req.body.Namsinh,
    DiemMon1: req.body.DiemMon1,
    DiemMon2: req.body.DiemMon2,
    DiemMon3: req.body.DiemMon3,
    Email: req.body.Email,
    DienThoai: req.body.DienThoai
  });

  sinhvien = await sinhvien.save();
  res.redirect('/');
  next();
});
```
{{< linebreak >}}
U (Update) Cập nhật thông tin sinh viên qua param id
```ts
router.put('/:id', async (req, res, next) => {
  const { error } = validateSinhVien(req.body);
  if (error) return res.status(400).send(error.details[0].message);

  const sinhvien = await SinhVien.findByIdAndUpdate(req.params.id, {
    MSSV: req.body.MSSV,
    Hoten: req.body.Hoten,
    Namsinh: req.body.Namsinh,
    DiemMon1: req.body.DiemMon1,
    DiemMon2: req.body.DiemMon2,
    DiemMon3: req.body.DiemMon3,
    Email: req.body.Email,
    DienThoai: req.body.DienThoai
  }, { new: true });

  if (!sinhvien) return res.status(404).send(`Sinh vien co ma ${req.params.id} khong ton tai`);
  res.redirect('/');
  next();
});
```
{{< linebreak >}}
D (Delete): xoá sinh viên thông qua id
```ts
router.delete('/:id', async (req, res, next) => {
  const sinhvien = await SinhVien.findByIdAndRemove(req.params.id);

  if (!sinhvien) return res.status(404).send(`Sinh vien co ma ${req.params.id} khong ton tai`);
  res.redirect('/');
  next();
});
module.exports = router;
```

{{< linebreak >}}
- Tạo view để hiển thị dữ liệu: ta sử dụng pug engine
- views/content/add.pug
```pug
h2 Thêm sinh viên
form.mb-5.pb-5(action='/' method='POST' target='_blank')
  .mb-3.row
    label.col-sm-1.col-form-label(for="MSSV") MSSV
    .col-sm-3
      input(type='text' name='MSSV')
  
  .mb-3.row
    label.col-sm-1.col-form-label(for="Hoten") Hoten
    .col-sm-3
      input(type='text' name='Hoten')
  
  .mb-3.row
    label.col-sm-1.col-form-label(for="Namsinh") NamSinh
    .col-sm-3
      input(type='text' name='Namsinh')

  .mb-3.row
    label.col-sm-1.col-form-label(for="DiemMon1") Diemmon1
    .col-sm-3
      input(type='text' name='DiemMon1')

  .mb-3.row
    label.col-sm-1.col-form-label(for="DiemMon2") Diemmon2
    .col-sm-3
      input(type='text' name='DiemMon2')

  .mb-3.row
    label.col-sm-1.col-form-label(for="DiemMon3") Diemmon3
    .col-sm-3
      input(type='text' name='DiemMon3')

  .mb-3.row
    label.col-sm-1.col-form-label(for="Email") Email
    .col-sm-3
      input(type='text' name='Email')

  .mb-3.row
    label.col-sm-1.col-form-label(for="DienThoai") DienThoai
    .col-sm-3
      input(type='text' name='DienThoai')
  
  input.btn.btn-primary(type='submit' value='Thêm')
```

{{< linebreak >}}
- views/content/index.pug
```pug
h2 Danh sách sinh viên
table.table
  thead
    tr
      th(scope="col") MSSV
      th(scope="col") Hoten
      th(scope="col") NamSinh
      th(scope="col") DiemMon1
      th(scope="col") DiemMon2
      th(scope="col") DiemMon3
      th(scope="col") Email
      th(scope="col") DienThoai
      th(scope="col") Sửa
      th(scope="col") Xoá
  tbody
    each sv in sinhvien
      tr
        th(scope="row")= sv.MSSV
        td= sv.Hoten
        td= sv.Namsinh
        td= sv.DiemMon1
        td= sv.DiemMon2
        td= sv.DiemMon3
        td= sv.Email
        td= sv.DienThoai
        td
          button.btn.btn-primary 
            a.link-light(href=`/${sv._id}`) Sửa sv
        td
          //- button.btn.btn-primary Xoá
          form(method='POST' action=`/${sv._id}?_method=DELETE`)
            button.btn.btn-primary(type='submit') Xoá sv
```

{{< linebreak >}}
- views/content/update.pug
```pug
h2 Cập nhật sinh viên
form.mb-5.pb-5(method='POST' action=`/${sinhvien._id}?_method=PUT` target='_blank')
  .mb-3.row
    label.col-sm-1.col-form-label(for="MSSV") MSSV
    .col-sm-3
      input(type='text' name='MSSV' value= sinhvien.MSSV)
  
  .mb-3.row
    label.col-sm-1.col-form-label(for="Hoten") Hoten
    .col-sm-3
      input(type='text' name='Hoten' value= sinhvien.Hoten)
  
  .mb-3.row
    label.col-sm-1.col-form-label(for="Namsinh") NamSinh
    .col-sm-3
      input(type='text' name='Namsinh' value= sinhvien.Namsinh)

  .mb-3.row
    label.col-sm-1.col-form-label(for="DiemMon1") Diemmon1
    .col-sm-3
      input(type='text' name='DiemMon1' value= sinhvien.DiemMon1)

  .mb-3.row
    label.col-sm-1.col-form-label(for="DiemMon2") Diemmon2
    .col-sm-3
      input(type='text' name='DiemMon2' value= sinhvien.DiemMon2)

  .mb-3.row
    label.col-sm-1.col-form-label(for="DiemMon3") Diemmon3
    .col-sm-3
      input(type='text' name='DiemMon3' value= sinhvien.DiemMon3)

  .mb-3.row
    label.col-sm-1.col-form-label(for="Email") Email
    .col-sm-3
      input(type='text' name='Email' value= sinhvien.Email)

  .mb-3.row
    label.col-sm-1.col-form-label(for="DienThoai") DienThoai
    .col-sm-3
      input(type='text' name='DienThoai' value= sinhvien.DienThoai)
  
  input.btn.btn-primary(type='submit' value='Cập nhật')
```

{{< linebreak >}}
- views/layout/index.pug
```pug
doctype html
head
  meta(charset='utf-8')
  meta(name='viewport' content='width=device-width, initial-scale=1')
  link(href='https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-alpha3/dist/css/bootstrap.min.css' rel='stylesheet' integrity='sha384-CuOF+2SnTUfTwSZjCXf01h7uYhfOBuxIhGKPbfEJ3+FqH/s6cIFN9bGr1HmAg4fQ' crossorigin='anonymous')
  link(rel="stylesheet", href="/css/styles.css")

  title Lab 9 & Lab 10
  body
    block content
    script(src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-alpha3/dist/js/bootstrap.bundle.min.js")
```

{{< linebreak >}}
- views/index.pug
```pug
extends ./layout/index

block content
  //- header
  include ./layout/header
  .container.mt-5.pt-5
    //- content
    include ./content/index

    //- post
    include ./content/add
```

{{< linebreak >}}
- views/change.pug
```pug
extends ./layout/index

block content
  //- header
  include ./layout/header
  .container.mt-5.pt-5
    //- content
    include ./content/update
```

{{< linebreak >}}
- Kết quả
- Hiển thị danh sách sinh viên
{{< figure src="/img/heqtcsdl/lab9-10/12.png" title="lab10" >}}
- Thêm sinh viên nào đó vd: Tran dinh quang
{{< figure src="/img/heqtcsdl/lab9-10/13.png" title="lab10" >}}
{{< figure src="/img/heqtcsdl/lab9-10/14.png" title="lab10" >}}
- Cập nhật sinh viên vd: tran dinh quang
{{< figure src="/img/heqtcsdl/lab9-10/15.png" title="lab10" >}}
{{< figure src="/img/heqtcsdl/lab9-10/16.png" title="lab10" >}}
- xoá sinh viên vd: dinh quang
{{< figure src="/img/heqtcsdl/lab9-10/17.png" title="lab10" >}}