# Chương 1. Controllers

## 1.1 Mục tiêu chương học

Sau khi hoàn thành chương này, bạn sẽ hiểu:

- Controller là gì trong NestJS.
- Vai trò của Controller trong kiến trúc ứng dụng.
- Cách NestJS thực hiện Request Routing.
- Cách sử dụng các HTTP Method Decorator.
- Cách nhận dữ liệu từ URL, Query String, Request Body và Header.
- Cách trả dữ liệu về Client.
- Các Best Practices khi xây dựng Controller.

---

## 1.2 Controller là gì?

Controller là thành phần chịu trách nhiệm tiếp nhận các HTTP Request từ Client và trả về HTTP Response.

Trong NestJS, Controller đóng vai trò là điểm vào (Entry Point) của ứng dụng.

Khi một Request được gửi tới hệ thống, NestJS sẽ xác định Controller phù hợp và gọi phương thức tương ứng để xử lý.

Ví dụ:

```text
Client
   ↓
Controller
   ↓
Provider (Service)
   ↓
Database
```

Controller không nên chứa nghiệp vụ phức tạp.

Nhiệm vụ chính của Controller bao gồm:

- Nhận Request
- Trích xuất dữ liệu từ Request
- Gọi Provider hoặc Service
- Trả Response về Client

---

## 1.3 Controller đầu tiên

Một Controller cơ bản trong NestJS:

```typescript
import { Controller, Get } from '@nestjs/common';

@Controller('users')
export class UsersController {

  @Get()
  findAll() {
    return 'Hello NestJS';
  }

}
```

Phân tích:

### @Controller('users')

Khai báo class là một Controller.

```typescript
@Controller('users')
```

Chuỗi `users` trở thành Route Prefix của toàn bộ Controller.

---

### @Get()

Khai báo một HTTP GET Endpoint.

```typescript
@Get()
```

Endpoint thực tế:

```http
GET /users
```

---

### findAll()

Phương thức được thực thi khi Client gửi Request.

```typescript
findAll() {
  return 'Hello NestJS';
}
```

Response:

```json
"Hello NestJS"
```

---

## 1.4 Cách NestJS khám phá Controller

Khi ứng dụng khởi động:

```typescript
await NestFactory.create(AppModule);
```

NestJS sẽ:

1. Quét toàn bộ Module.
2. Tìm các Controller đã đăng ký.
3. Phân tích Metadata từ Decorator.
4. Tạo Routing Table nội bộ.

Ví dụ:

```typescript
@Module({
  controllers: [UsersController]
})
export class UsersModule {}
```

Controller chỉ hoạt động khi được khai báo trong Module.

---

## 1.5 Request Routing

Routing là cơ chế ánh xạ URL tới một phương thức trong Controller.

Ví dụ:

```typescript
@Controller('users')
export class UsersController {

  @Get()
  findAll() {}

  @Post()
  create() {}

}
```

NestJS tự động tạo:

```http
GET  /users
POST /users
```

Khi Request:

```http
GET /users
```

được gửi tới hệ thống:

```text
Request
   ↓
UsersController.findAll()
```

Khi Request:

```http
POST /users
```

được gửi tới hệ thống:

```text
Request
   ↓
UsersController.create()
```

---

## 1.6 Route Prefix

NestJS cho phép khai báo tiền tố Route ở cấp Controller.

Ví dụ:

```typescript
@Controller('users')
export class UsersController {}
```

Mọi Endpoint bên trong Controller sẽ được gắn tiền tố:

```text
/users
```

Ví dụ:

```typescript
@Controller('users')
export class UsersController {

  @Get()
  findAll() {}

  @Get(':id')
  findOne() {}

}
```

Kết quả:

```http
GET /users
GET /users/:id
```

---

## 1.7 HTTP Request Methods

NestJS hỗ trợ đầy đủ HTTP Method Decorator.

### GET

```typescript
@Get()
findAll() {}
```

```http
GET /users
```

Dùng để lấy dữ liệu.

---

### POST

```typescript
@Post()
create() {}
```

```http
POST /users
```

Dùng để tạo dữ liệu mới.

---

### PUT

```typescript
@Put(':id')
replace() {}
```

```http
PUT /users/1
```

Thay thế toàn bộ tài nguyên.

---

### PATCH

```typescript
@Patch(':id')
update() {}
```

```http
PATCH /users/1
```

Cập nhật một phần dữ liệu.

---

### DELETE

```typescript
@Delete(':id')
remove() {}
```

```http
DELETE /users/1
```

Xóa dữ liệu.

---

### OPTIONS

```typescript
@Options()
options() {}
```

Dùng trong CORS và API Discovery.

---

### HEAD

```typescript
@Head()
head() {}
```

Trả về Header mà không trả Body.

---

### ALL

```typescript
@All()
handler() {}
```

Nhận mọi HTTP Method.

---

## 1.8 Route Parameters

Route Parameter cho phép truyền dữ liệu trực tiếp trong URL.

Ví dụ:

```http
GET /users/100
```

Controller:

```typescript
@Get(':id')
findOne(@Param('id') id: string) {
  return id;
}
```

Response:

```json
"100"
```

---

### Nhiều Parameters

```typescript
@Get(':userId/orders/:orderId')
findOrder(
  @Param('userId') userId: string,
  @Param('orderId') orderId: string
) {
  return { userId, orderId };
}
```

Request:

```http
GET /users/1/orders/10
```

Response:

```json
{
  "userId": "1",
  "orderId": "10"
}
```

---

## 1.9 ParseIntPipe

Route Parameter mặc định l# Chương 1. Controllers

## 1.1 Mục tiêu chương học

Sau khi hoàn thành chương này, bạn sẽ hiểu:

- Controller là gì trong NestJS.
- Vai trò của Controller trong kiến trúc ứng dụng.
- Cách NestJS thực hiện Request Routing.
- Cách xây dựng các HTTP Endpoint.
- Cách nhận dữ liệu từ URL, Query String, Request Body và Header.
- Cách trả dữ liệu về Client.
- Các Best Practices khi thiết kế Controller.

---

## 1.2 Controller là gì?

Controller là thành phần chịu trách nhiệm tiếp nhận các HTTP Request từ Client và trả về HTTP Response.

Trong NestJS, Controller là điểm tiếp xúc đầu tiên giữa ứng dụng và thế giới bên ngoài.

Ví dụ:

```text
Client
   ↓
Controller
   ↓
Provider
   ↓
Database
```

Khi Client gửi Request:

```http
GET /users
```

NestJS sẽ:

1. Xác định Controller phù hợp.
2. Tìm phương thức xử lý tương ứng.
3. Thực thi phương thức đó.
4. Trả kết quả về Client.

Một Controller nên tập trung vào:

- Nhận Request
- Trích xuất dữ liệu
- Gọi Service
- Trả Response

Controller không nên chứa Business Logic phức tạp.

---

## 1.3 Tạo Controller đầu tiên

Nest CLI có thể tự động sinh Controller:

```bash
nest generate controller users
```

Hoặc:

```bash
nest g co users
```

Sau khi thực thi:

```text
src/
└── users/
    ├── users.controller.ts
    └── users.controller.spec.ts
```

Ví dụ Controller cơ bản:

```typescript
import { Controller, Get } from '@nestjs/common';

@Controller('users')
export class UsersController {

  @Get()
  findAll() {
    return 'Hello NestJS';
  }

}
```

Khi chạy ứng dụng:

```bash
npm run start:dev
```

Truy cập:

```http
GET /users
```

Kết quả:

```json
"Hello NestJS"
```

---

## 1.4 Decorator @Controller()

Decorator `@Controller()` được sử dụng để đánh dấu một class là Controller.

Ví dụ:

```typescript
@Controller()
export class AppController {}
```

Controller trên không có Route Prefix.

---

Có thể khai báo Route Prefix:

```typescript
@Controller('users')
export class UsersController {}
```

Khi đó toàn bộ Route trong Controller sẽ được gắn tiền tố:

```text
/users
```

Ví dụ:

```typescript
@Controller('users')
export class UsersController {

  @Get()
  findAll() {}

}
```

Endpoint thực tế:

```http
GET /users
```

---

## 1.5 Request Routing

Routing là quá trình ánh xạ URL tới một phương thức trong Controller.

Ví dụ:

```typescript
@Controller('users')
export class UsersController {

  @Get()
  findAll() {}

  @Post()
  create() {}

}
```

NestJS tự động tạo:

```http
GET /users
POST /users
```

Khi Request:

```http
GET /users
```

được gửi tới hệ thống:

```text
UsersController.findAll()
```

sẽ được thực thi.

Khi Request:

```http
POST /users
```

được gửi tới hệ thống:

```text
UsersController.create()
```

sẽ được thực thi.

---

## 1.6 HTTP Request Methods

NestJS hỗ trợ đầy đủ các HTTP Method thông qua Decorator.

### GET

```typescript
@Get()
findAll() {}
```

```http
GET /users
```

Thường dùng để lấy dữ liệu.

---

### POST

```typescript
@Post()
create() {}
```

```http
POST /users
```

Thường dùng để tạo dữ liệu.

---

### PUT

```typescript
@Put(':id')
replace() {}
```

```http
PUT /users/1
```

Thay thế toàn bộ dữ liệu của tài nguyên.

---

### PATCH

```typescript
@Patch(':id')
update() {}
```

```http
PATCH /users/1
```

Cập nhật một phần dữ liệu.

---

### DELETE

```typescript
@Delete(':id')
remove() {}
```

```http
DELETE /users/1
```

Xóa dữ liệu.

---

### OPTIONS

```typescript
@Options()
options() {}
```

Được sử dụng trong một số trường hợp liên quan tới CORS.

---

### HEAD

```typescript
@Head()
head() {}
```

Trả về Header mà không trả Body.

---

### ALL

```typescript
@All()
handler() {}
```

Xử lý mọi HTTP Method.

---

## 1.7 Route Parameters

Route Parameter cho phép truyền dữ liệu trực tiếp trên URL.

Ví dụ:

```http
GET /users/100
```

Controller:

```typescript
@Get(':id')
findOne(
  @Param('id')
  id: string
) {
  return id;
}
```

Response:

```json
"100"
```

---

### Nhiều Parameters

```typescript
@Get(':userId/orders/:orderId')
findOrder(
  @Param('userId') userId: string,
  @Param('orderId') orderId: string
) {
  return {
    userId,
    orderId
  };
}
```

Request:

```http
GET /users/1/orders/10
```

Response:

```json
{
  "userId": "1",
  "orderId": "10"
}
```

---

## 1.8 Query Parameters

Query Parameters thường được sử dụng cho:

- Pagination
- Search
- Filtering
- Sorting

Ví dụ:

```http
GET /users?page=1&limit=10
```

Controller:

```typescript
@Get()
findAll(
  @Query('page') page: string,
  @Query('limit') limit: string
) {
  return {
    page,
    limit
  };
}
```

Response:

```json
{
  "page": "1",
  "limit": "10"
}
```

---

## 1.9 Request Body

Request Body thường xuất hiện trong:

- POST
- PUT
- PATCH

Ví dụ:

```json
{
  "name": "John",
  "email": "john@example.com"
}
```

Controller:

```typescript
@Post()
create(
  @Body()
  body: any
) {
  return body;
}
```

Response:

```json
{
  "name": "John",
  "email": "john@example.com"
}
```

---

## 1.10 Request Headers

Đọc Header từ Request:

```typescript
@Get()
findAll(
  @Headers('authorization')
  authorization: string
) {
  return authorization;
}
```

Request:

```http
GET /users
Authorization: Bearer token
```

Response:

```json
"Bearer token"
```

Một số Header phổ biến:

- Authorization
- Content-Type
- Accept
- User-Agent

---

## 1.11 Request Object

NestJS cho phép truy cập Request gốc.

```typescript
@Get()
findAll(
  @Req() request: Request
) {
  return request.headers;
}
```

Decorator:

```typescript
@Req()
```

trả về Request Object của nền tảng bên dưới.

Ví dụ:

- Express
- Fastify

Thông thường nên ưu tiên sử dụng:

- @Param()
- @Query()
- @Body()
- @Headers()

thay vì truy cập trực tiếp Request Object.

---

## 1.12 Response Handling

NestJS tự động chuyển đổi dữ liệu trả về thành JSON.

Ví dụ:

```typescript
@Get()
findAll() {
  return [
    'user1',
    'user2'
  ];
}
```

Response:

```json
[
  "user1",
  "user2"
]
```

NestJS sẽ tự động:

- Serialize dữ liệu
- Chuyển sang JSON
- Trả HTTP Response

---

## 1.13 HTTP Status Codes

Mặc định:

```http
GET -> 200 OK
POST -> 201 Created
```

Có thể thay đổi:

```typescript
@HttpCode(204)
@Delete(':id')
remove() {}
```

Response:

```http
204 No Content
```

NestJS cũng cung cấp enum:

```typescript
HttpStatus
```

Ví dụ:

```typescript
@HttpCode(HttpStatus.OK)
```

---

## 1.14 Redirect

NestJS hỗ trợ Redirect.

```typescript
@Redirect('https://nestjs.com')
@Get()
redirect() {}
```

Response:

```http
302 Found
Location: https://nestjs.com
```

---

## 1.15 Async Controllers

Controller có thể xử lý bất đồng bộ.

```typescript
@Get()
async findAll() {
  return await this.userService.findAll();
}
```

NestJS tự động xử lý Promise.

Không cần:

```typescript
.then()
.catch()
```

trong phần lớn trường hợp.

---

## 1.16 Best Practices

### Controller nên mỏng

Tốt:

```typescript
@Get()
findAll() {
  return this.userService.findAll();
}
```

Không tốt:

```typescript
@Get()
findAll() {

  // validate

  // calculate

  // query database

  // send email

}
```

---

### Một Controller cho một Domain

Ví dụ:

```text
users.controller.ts
products.controller.ts
orders.controller.ts
```

Không nên gom toàn bộ endpoint vào một Controller duy nhất.

---

### Sử dụng DTO

Không nên:

```typescript
@Body() body: any
```

Ưu tiên:

```typescript
@Body() dto: CreateUserDto
```

---

## 1.17 Common Mistakes

### Business Logic trong Controller

Controller chỉ nên điều phối luồng xử lý.

Business Logic nên nằm trong Provider hoặc Service.

---

### Truy cập Database trực tiếp

Không nên:

```typescript
@Get()
findAll() {
  return this.prisma.user.findMany();
}
```

Nên:

```typescript
@Get()
findAll() {
  return this.userService.findAll();
}
```

---

### Controller quá lớn

Nếu một Controller chứa quá nhiều endpoint hoặc quá nhiều logic, hãy xem xét tách theo Domain hoặc Responsibility.

---

## 1.18 Tóm tắt chương

Trong chương này chúng ta đã tìm hiểu:

- Controller
- Request Routing
- HTTP Methods
- Route Parameters
- Query Parameters
- Request Body
- Headers
- Request Object
- Response Handling
- HTTP Status Codes
- Redirect
- Async Controllers

Controller là điểm tiếp nhận Request của ứng dụng NestJS và là một trong những thành phần quan trọng nhất của framework.à String.

Ví dụ:

```typescript
@Get(':id')
findOne(@Param('id') id: string) {}
```

Nếu cần Number:

```typescript
@Get(':id')
findOne(
  @Param('id', ParseIntPipe)
  id: number
) {
  return id;
}
```

Request:

```http
GET /users/100
```

Response:

```json
100
```

Nếu:

```http
GET /users/abc
```

NestJS sẽ trả:

```http
400 Bad Request
```

---

## 1.10 Query Parameters

Query Parameter thường được dùng cho:

- Pagination
- Search
- Filter
- Sort

Ví dụ:

```http
GET /users?page=1&limit=10
```

Controller:

```typescript
@Get()
findAll(
  @Query('page') page: string,
  @Query('limit') limit: string
) {
  return { page, limit };
}
```

Response:

```json
{
  "page": "1",
  "limit": "10"
}
```

---

## 1.11 Pagination Example

Ví dụ thực tế:

```typescript
@Get()
findAll(
  @Query('page') page = 1,
  @Query('limit') limit = 10
) {
  return this.userService.findAll(page, limit);
}
```

Request:

```http
GET /users?page=2&limit=20
```

---

## 1.12 Request Body

Request Body thường xuất hiện trong:

- POST
- PUT
- PATCH

Ví dụ:

```json
{
  "name": "John",
  "email": "john@example.com"
}
```

Controller:

```typescript
@Post()
create(@Body() body: any) {
  return body;
}
```

Response:

```json
{
  "name": "John",
  "email": "john@example.com"
}
```

---

## 1.13 DTO Introduction

Thay vì dùng:

```typescript
@Body() body: any
```

Nên sử dụng DTO.

```typescript
export class CreateUserDto {
  name: string;
  email: string;
}
```

Controller:

```typescript
@Post()
create(
  @Body()
  dto: CreateUserDto
) {
  return dto;
}
```

DTO sẽ được tìm hiểu chi tiết ở chương riêng.

---

## 1.14 Request Headers

Đọc Header:

```typescript
@Get()
findAll(
  @Headers('authorization')
  authorization: string
) {
  return authorization;
}
```

Request:

```http
GET /users
Authorization: Bearer token
```

Response:

```json
"Bearer token"
```

---

## 1.15 Request Object

NestJS cho phép truy cập Request gốc.

```typescript
@Get()
findAll(@Req() request: Request) {
  return request.headers;
}
```

Decorator:

```typescript
@Req()
```

trả về Request Object của nền tảng bên dưới.

Ví dụ:

- Express
- Fastify

---

## 1.16 Response Handling

Thông thường NestJS tự động tạo Response.

```typescript
@Get()
findAll() {
  return ['user1', 'user2'];
}
```

Response:

```json
[
  "user1",
  "user2"
]
```

NestJS tự động:

- Serialize dữ liệu
- Chuyển sang JSON
- Trả Status Code phù hợp

---

## 1.17 HTTP Status Code

GET mặc định:

```http
200 OK
```

POST mặc định:

```http
201 Created
```

Có thể thay đổi:

```typescript
@HttpCode(204)
@Delete(':id')
remove() {}
```

Response:

```http
204 No Content
```

---

## 1.18 Redirect

NestJS hỗ trợ Redirect.

```typescript
@Redirect('https://nestjs.com')
@Get()
redirect() {}
```

Response:

```http
302 Found
Location: https://nestjs.com
```

---

## 1.19 Async Controllers

Controller có thể xử lý bất đồng bộ.

```typescript
@Get()
async findAll() {
  return await this.userService.findAll();
}
```

NestJS tự động xử lý Promise.

Không cần gọi:

```typescript
.then()
.catch()
```

---

## 1.20 Best Practices

### Controller nên mỏng

Tốt:

```typescript
@Get()
findAll() {
  return this.userService.findAll();
}
```

Không nên:

```typescript
@Get()
findAll() {

  const users = [];

  // business logic 300 dòng

  return users;
}
```

---

### Một Controller cho một Domain

Ví dụ:

```text
users.controller.ts
products.controller.ts
orders.controller.ts
```

Không nên gom toàn bộ Endpoint vào một Controller.

---

### Luôn sử dụng DTO

Không dùng:

```typescript
@Body() body: any
```

Ưu tiên:

```typescript
@Body() dto: CreateUserDto
```

---

## 1.21 Common Mistakes

### Business Logic trong Controller

Sai:

```typescript
@Post()
create() {

  // validate
  // calculate
  // query database
  // send email

}
```

---

### Truy cập Database trực tiếp

Sai:

```typescript
@Get()
findAll() {
  return this.prisma.user.findMany();
}
```

Nên:

```typescript
@Get()
findAll() {
  return this.userService.findAll();
}
```

---

### Controller quá lớn

Nếu Controller vượt quá vài trăm dòng, nên xem xét tách theo Domain hoặc Responsibility.

---

## 1.22 Tóm tắt chương

Controller là điểm tiếp nhận Request của ứng dụng NestJS.

Trong chương này chúng ta đã tìm hiểu:

- Controller
- Routing
- HTTP Methods
- Route Parameters
- Query Parameters
- Request Body
- Headers
- Request Object
- Response Handling
- Status Code
- Redirect
- Async Controller
- Best Practices

Đây là nền tảng quan trọng trước khi chuyển sang chương tiếp theo: **Providers**.