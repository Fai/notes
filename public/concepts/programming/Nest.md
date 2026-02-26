pattern

`run with auto reload`
```
yarn start:dev
```
จะสามารถทดสอบได้ที่ `localhost:3000`

`nest new project-name`

Project Structure
```

```
ตั้งค่า prefix ที่ main.ts
```
async function bootstrap() {
	const app = await NestFactory.create(AppModule);
	app.setGlobalPrefix('api')
	await app.listen(3000);
}
bootstrap();
```


nest generate module module-name
```

```
nest generate controller controller-name

ใส่ decorator ตามประเภทของ http request ที่มีการเรียก
```
@Controller('Product')
export class ProductController {
	constructor(private productService: ProductService)
	@Get
	getProductAll(@Query('name') productName ?: string): ProductDTO[] {
		if (productName) {
			return this.productService.findByCondition((product) =>
				product.name.includes(productName),
			);
		}
		return this.productService.findAll()
	}
	
	@Get(':id')
	getProductByID(@Param('id') id: string){
		return this.productService.findById(Number(id)); 
	}
}
```
nest generate service service-name
ฝั่ง Business Logic
```
@Injectable()
export class ProductService {
	private products: ProductDTO[] = [];
	findAll(): ProductDTO[] {
		return this.products;
	}
	
	findById(id: number){
		return this.products.find((p) => p.id === id)
	}
	
	findByCondition(predicate: (product: ProductDTO) => boolean){
		return this.products.filter((p) => predicate(p))
	}
}
```
ตัวอย่าง query parameters