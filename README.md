<h1 align="center">Project: Website bán đồ chơi</h1>

## 👤 Thông Tin Cá Nhân  
- **Họ tên**: Hoàng Minh Quân 
- **Mã sinh viên**: 23010315
- **Lớp**: CNTT_4
- **Môn học**: Xây dựng web nâng cao (TH3)

## 📈 Mục đích dự án
- Xây dựng một website bán đồ chơi trực tuyến nhằm giúp khách hàng dễ dàng xem thông tin chi tiết về các sản phẩm đồ chơi, đặt hàng và thanh toán một cách nhanh chóng, tiện lợi.
- Hỗ trợ chủ cửa hàng quản lý hiệu quả các danh mục như sản phẩm, khách hàng, đơn hàng, doanh thu và các hóa đơn thanh toán, từ đó tối ưu hóa hoạt động kinh doanh.
- Website không chỉ là nền tảng giao dịch, mà còn là nơi cung cấp thông tin hữu ích về các loại đồ chơi, giúp khách hàng nắm bắt xu hướng mới nhất và lựa chọn sản phẩm phù hợp.

## ⚙ Hệ thống sử dụng
- PHP (Laravel framework)
- Laravel Breeze
- MySQL (Aiven Cloud)
- Eloquent ORM (Hệ thống ORM giúp tương tác với CSDL)
- Frontend & UI (Blade engine, Tailwind CSS, Bootstrap 5)
- Laravel Security (Framework hỗ trợ)
- AJAX JQuery (Phục vụ tìm kiếm)

## ⚙️ Sơ đồ chức năng
Sơ đồ tổng quát

![image](https://github.com/user-attachments/assets/1dfc6136-3b10-4af2-83b4-18041d12b790)

Sử dụng hệ thống

![image](https://github.com/user-attachments/assets/71cac250-43ac-40d6-a07a-fad70002eb53)

Quản lý sản phẩm

![image](https://github.com/user-attachments/assets/ab3d67da-55a4-4848-bbab-50f204b7c683)

Quản lý tài khoản

![image](https://github.com/user-attachments/assets/92c500e9-1c8d-45a2-aa9c-b9cb5bf4ba73)

Quản lý đơn đặt hàng

![image](https://github.com/user-attachments/assets/0f445197-a978-4d1a-b25e-992799b86d22)

## 📊 Sơ đồ tuần tự
Đăng kí tài khoản

![image](https://github.com/user-attachments/assets/2b52f45a-b08a-4715-90a9-51eba4b0eca0)

Đăng nhập

![image](https://github.com/user-attachments/assets/a706f820-92ef-4edb-afda-8629857fb8d1)

Thêm sản phẩm vào giỏ hàng

![image](https://github.com/user-attachments/assets/2fe7d86f-22cb-47e6-ba81-18c04355af98)

Thanh toán

![image](https://github.com/user-attachments/assets/0af82e00-71de-45b9-a150-4c4a0b12babf)

Cập nhật thông tin

![image](https://github.com/user-attachments/assets/7a079a19-7a2f-4ae2-a83d-759a9f90dc76)

## Sơ đồ khối 

![image](https://github.com/user-attachments/assets/89a53a6e-95c6-4b29-9e53-34df520a9827)

<h1>Một số code minh họa</h1>

## Model

#### Order Model 
```php
// Model đơn đặt hàng
class Orders extends Model
{
    use HasFactory;

    protected $fillable = [
        'orders_id',
        'orders_users_id',
        'orders_product',
        'orders_quantity',
        'orders_price',
        'orders_censor',
        'orders_phonenumber',
        'orders_address',
    ];

    public function user()
    {
        return $this->belongsTo(User::class, 'orders_users_id');
    }

    public function ship()
    {
        return $this->hasMany(Shipper::class);
    }

    // public function ac_order(){
    //     return $this->belongsTo(AC_order::class , 'orders_id');
    // }
}
```

#### Product Model

``` php
// Model sản phẩm
class Product extends Model
{
    use HasFactory;

    protected $fillable = [
        'product_name',
        'product_price',
        'product_cat_id',
        'product_subcat_id',
        'product_attribute_id',
        'product_status',
        'product_quantity',
        'product_img'
    ];


    // các phần từ con kết nối đến phần tử cha là category , subcategory , default_attribute
    public function category(){
        return $this->belongsTo(Category::class, 'product_cat_id');
    }

    public function subcategory(){
        return $this->belongsTo(Subcategory::class, 'product_subcat_id');
    }

    public function default_attribute(){
        return $this->belongsTo(DefaultAttribute::class, 'product_attribute_id');
    }

    // phần tử cha kết nối vs AC_order
    public function AC_order(){
        return $this->hasMany(AC_order::class);
    }

    public function cartcus(){
        return $this->hasMany(CartCus::class);
    }

}
```

#### Shipper Model

``` php
// Model Shipper
class Shipper extends Model
{
    use HasFactory;

    protected $fillable = [
        'ship_orders_id',
        'ship_users',
        'ship_product',
        'ship_quantity',
        'ship_price',
        'ship_phonenumber',
        'ship_address',
        'ship_thank'
    ];

    public function order()
    {
        return $this->belongsTo(Orders::class, 'ship_orders_id');
    }
}
```

#### User Model

``` php
// Model User
class User extends Authenticatable
{
    /** @use HasFactory<\Database\Factories\UserFactory> */
    use HasFactory, Notifiable;

    /**
     * The attributes that are mass assignable.
     *
     * @var array<int, string>
     */
    protected $fillable = [
        'name',
        'email',
        'role',
        'img_user',
        'password',
    ];

    /**
     * The attributes that should be hidden for serialization.
     *
     * @var array<int, string>
     */
    protected $hidden = [
        'password',
        'remember_token',
    ];

    /**
     * Get the attributes that should be cast.
     *
     * @return array<string, string>
     */
    protected function casts(): array
    {
        return [
            'email_verified_at' => 'datetime',
            'password' => 'hashed',
        ];
    }

    public function orders(){
        return $this->hasMany(Orders::class);
    }

    public function cartcus(){
        return $this->hasMany(CartCus::class);
    }
}
```

#### Category Model

``` php
// Model danh mục chính
class Category extends Model
{
    use HasFactory;

    protected $fillable = [
        'category_name'
    ];

    // phần tử cha kết nối đến con là subcategory
    public function subcategory(){
        return $this->hasMany(Subcategory::class);
    }

    // phần tử cha kết nối đến con là product
    public function product(){
        return $this->hasMany(Product::class);
    }
}
```

#### Model Subcategory

``` php
// Model danh mục phụ
class Subcategory extends Model
{
    use HasFactory;

    protected $fillable = [
        'subcategory_name',
        'category_id'
    ];


    // phần tử con kết nối đến cha là category
    public function category(){
        return $this->belongsTo(Category::class , 'category_id');
    }

    // phần tử cha kết nối đến con là product
    public function product(){
        return $this->hasMany(Product::class);
    }
}
```

#### Model CartCus

``` php
// Model Giỏ hàng customer
class CartCus extends Model
{
    use HasFactory;
    protected $fillable = [
        'product_id',
        'user_id',
        'cart_quantity',
        'cart_price',
    ];

    public function product(){
        return $this->belongsTo(Product::class,'product_id');
    }

    public function user(){
        return $this->belongsTo(User::class,'user_id');
    }
}
```

## Controller

#### Order Controller

``` php
    // Manage
    public function manage(){
        $orders = Orders::all()->sortDesc();
        return view('seller.store.history' , compact('orders'));
    }

    // Read
    public function order_edit($id)
    {
        $orders_info = Orders::find($id);
        return view('seller.store.edit_order', compact('orders_info'));
    }

    // Update
    public function order_update(Request $request, $id)
    {
        $order_edit = Orders::findOrFail($id);

        $request->validate([
            'orders_status' => 'required|string|max:255',
            'orders_phonenumber' => 'nullable|string|max:15', // Đổi thành kiểu string nếu số điện thoại phức tạp
            'orders_address' => 'nullable|string|max:255',
        ]);

        try {
            $order_edit->update([
                'orders_censor' => $request->orders_status,
                'orders_phonenumber' => ($request->orders_phonenumber == 0) ? "0" : $request->orders_phonenumber,
                'orders_address' => ($request->orders_address === "Trống") ? "Trống" : $request->orders_address,
            ]);

            return redirect()->route('vendor.store.manage')->with('success', "Cập nhật đơn hàng $order_edit->id thành công!");
        } catch (Exception $e) {
            return redirect()->back()->with('error', "Lỗi cập nhật đơn hàng: " . $e->getMessage());
        }
    }

    // Search
    public function order_search(Request $request)
    {
        $searchTerm = $request->input('search_orders');

        $orders = Orders::where('orders_id', 'like', "%$searchTerm%")
            ->orWhere('orders_product', 'like', "%$searchTerm%")
            ->orWhere('orders_quantity', 'like', "%$searchTerm%")
            ->orWhere('orders_price', 'like', "%$searchTerm%")
            ->orWhere('orders_censor', 'like', "%$searchTerm%")
            ->orWhere('created_at', 'like', "%$searchTerm%")
            ->orWhereHas('user', function ($query) use ($searchTerm) {
                $query->where('name', 'like', "%$searchTerm%");
            })
            ->get();

        if ($orders->isEmpty()) {
            return response('<tr class="alert alert-danger"><td colspan="10">Không tìm thấy đơn hàng này</td></tr>');
        } else {
            $output = '';
            foreach ($orders as $order) {
                $censorClass = '';
                if ($order->orders_censor == "Đang kiểm duyệt") {
                    $censorClass = '<div class="bg-warning bg-gradient bg-opacity-75 text-center rounded-pill p-1">' . $order->orders_censor . '</div>';
                } elseif ($order->orders_censor == "Đã kiểm duyệt") {
                    $censorClass = '<div class="bg-success bg-gradient bg-opacity-75 text-center rounded-pill p-1">' . $order->orders_censor . '</div>';
                } else {
                    $censorClass = $order->orders_censor;
                }

                $output .= '
                <tr>
                    <td>' . $order->id . '</td>
                    <td>' . $order->orders_id . '</td>
                    <td>' . ($order->user->name ?? "Không có") . '</td>
                    <td>' . $order->orders_product . '</td>
                    <td>' . $order->orders_quantity . '</td>
                    <td>' . $order->orders_price . '</td>
                    <td>' . $censorClass . '</td>
                    <td>' . $order->created_at . '</td>
                    <td>
                        <a href="' . '#' . '" class="btn btn-success"><i class="fa-solid fa-pen-to-square"></i></a>
                    </td>
                    <td>
                        <a href="' . '#' . '" class="btn btn-primary"><i class="fa-solid fa-truck-fast"></i></a>
                    </td>
                </tr>
            ';
            }
            return response($output);
        }
    }
```

#### Product Controller

``` php
    // Read
    public function index()
    {
        $products = Product::paginate(4);
        return view('admin/product/manage', compact('products'));
    }

    // Create
    public function productcreate()
    {
        $categories = Category::all();
        $subcategories = Subcategory::all();
        $attributes = DefaultAttribute::all();
        return view('admin.product.create_manage', compact('categories', 'subcategories', 'attributes'));
    }
    
    public function productinsert(Request $request)
    {
        $request->validate([
            'product_name' => 'required|string|max:255',
            'product_price' => 'required|numeric|min:0',
            'product_cat_name' => 'required|exists:categories,id',
            'product_subcat_name' => 'required|exists:subcategories,id',
            'product_attribute_name' => 'required|exists:default_attributes,id',
            'product_status' => 'required|string|max:255',
            'product_quantity' => 'required|integer',
            'product_img' => 'nullable|image|mimes:jpeg,png,jpg,gif,svg|max:2048'
        ]);

        try {
            $imagePath = 'admin_asset/img/photos/blocks.png'; // Đường dẫn mặc định nếu không tải lên ảnh mới

            if ($request->hasFile('product_img')) {
                $file = $request->file('product_img');
                $fileName = time() . '_' . $file->getClientOriginalName();
                $destinationPath = public_path('admin_asset/img/photos');
                $file->move($destinationPath, $fileName);
                $imagePath =  $fileName;
            }

            Product::create([
                'product_name' => $request->product_name,
                'product_price' => $request->product_price,
                'product_cat_id' => $request->product_cat_name,
                'product_subcat_id' => $request->product_subcat_name,
                'product_attribute_id' => $request->product_attribute_name,
                'product_status' => $request->product_status,
                'product_quantity' => $request->product_quantity,
                'product_img' => $imagePath,
            ]);

            return redirect()->route('product.manage')->with('success', 'Sản phẩm đã được thêm thành công.');
        } catch (Exception $e) {
            return redirect()->back()->with('error', 'Lỗi khi thêm sản phẩm: ' . $e->getMessage());
        }
    }

    // Delete
    public function productdelete($id)
    {
        $products = Product::findOrFail($id);

        try {
            $products->delete();
            return redirect()->route('product.manage')->with(['success' => "Xóa sản phẩm của ID $id thành công!"]);
        } catch (Exception $e) {
            echo $e->getMessage();
            return redirect()->back()->with('error', 'Lỗi , không xóa được sản phẩm!');
        }
    }

    // Update
    public function productedit($id)
    {
        $categories = Category::all();
        $subcategories = Subcategory::all();
        $attributes = DefaultAttribute::all();
        $product_info = Product::find($id);
        return view('admin.product.edit_manage', compact('categories', 'subcategories', 'attributes', 'product_info'));
    }
    public function productupdate(Request $request, $id)
    {
        $product = Product::findOrFail($id);

        $request->validate([
            'product_name' => 'required|string|max:255',
            'product_price' => 'required|numeric|min:0',
            'product_cat_name' => 'required|exists:categories,id',
            'product_subcat_name' => 'required|exists:subcategories,id',
            'product_attribute_name' => 'required|exists:default_attributes,id',
            'product_status' => 'required|string|max:255',
            'product_quantity' => 'required|integer',
            'product_img' => 'nullable|image|mimes:jpeg,png,jpg,gif,svg|max:2048'
        ]);

        try {
            // Đường dẫn mặc định cho ảnh nếu không có ảnh được tải lên
            $imagePath = $product->product_img ?? 'admin_asset/img/photos/blocks.png';

            // Kiểm tra nếu có file ảnh mới được tải lên
            if ($request->hasFile('product_img')) {
                $file = $request->file('product_img');
                $fileName = time() . '_' . $file->getClientOriginalName();
                $destinationPath = public_path('admin_asset/img/photos');
                $file->move($destinationPath, $fileName);
                $imagePath = $fileName;
            }

            // Cập nhật dữ liệu nhân viên vào cơ sở dữ liệu
            $product->update([
                'product_name' => $request->product_name,
                'product_price' => $request->product_price,
                'product_cat_id' => $request->product_cat_name,
                'product_subcat_id' => $request->product_subcat_name,
                'product_attribute_id' => $request->product_attribute_name,
                'product_status' => $request->product_status,
                'product_quantity' => $request->product_quantity,
                'product_img' => $imagePath,
            ]);

            return redirect()->route('product.manage')->with('success', "Sản phẩm ID $id đã được cập nhật thành công.");
        } catch (Exception $e) {
            return redirect()->back()->with('error', 'Lỗi cập nhật đơn hàng');
        }
    }

    public function productsearch(Request $request)
    {
        $searchTerm = $request->input('search_product');

        $products = Product::where('product_name', 'like', "%$searchTerm%")
            ->orWhere('product_price', 'like', "%$searchTerm%")
            ->orWhere('product_status', 'like', "%$searchTerm%")
            ->orWhere('product_quantity', 'like', "%$searchTerm%")
            ->orWhereHas('category', function ($query) use ($searchTerm) {
                $query->where('category_name', 'like', "%$searchTerm%");
            })
            ->orWhereHas('subcategory', function ($query) use ($searchTerm) {
                $query->where('subcategory_name', 'like', "%$searchTerm%");
            })
            ->orWhereHas('default_attribute', function ($query) use ($searchTerm) {
                $query->where('attribute_value', 'like', "%$searchTerm%");
            })
            ->get();

        if ($products->isEmpty()) {
            return response('<tr><td colspan="6">Không tìm thấy sản phẩm nào</td></tr>');
        } else {
            $output = '';
            foreach ($products as $product) {
                $imgPath = $product->product_img == null
                    ? asset('admin_asset/img/photos/blocks.png')
                    : asset("admin_asset/img/photos/{$product->product_img}");

                $output .= '
            <tr>
                <td>' . $product->product_name . '</td>
                <td>' . $product->product_price . '</td>
                <td>' . $product->category->category_name . '</td>
                <td>' . $product->subcategory->subcategory_name . '</td>
                <td>' . $product->default_attribute->attribute_value . '</td>
                <td>' . $product->product_status . '</td>
                <td>' . $product->product_quantity . '</td>
                <td>
                    <img src="' . $imgPath . '" alt="" class="img_user">
                </td>
                <td>
                    <a href="#" class="btn btn-success"><i class="fa-solid fa-pen-to-square"></i></a>
                </td>
                <td>
                    <form action="#" method="post" class="d-inline">
                        ' . csrf_field() . method_field('delete') . '
                        <button type="submit" class="btn btn-danger">
                            <i class="fa-solid fa-trash"></i>
                        </button>
                    </form>
                </td>
            </tr>';
            }
            return response($output);
        }
    }
```

#### Shipper Controller

``` php
    // Read
    public function ship(){
        $shipers = Shipper::all()->sortDesc();
        return view('seller.store.listship' , compact('shipers'));
    }

    public function cart_photos_pdf($id){
        $shiper_order = Shipper::find($id);

        $pdfOptions = [
            'defaultFont' => 'DejaVu Sans',
            'isHtml5ParserEnabled' => true,
            'isRemoteEnabled' => true,
        ];

        $pdf = Pdf::loadView('seller.store.inova', compact('shiper_order'))->setOptions($pdfOptions);

        return $pdf->download(($shiper_order->ship_users ?? "Ko có").time().".pdf");
        // return view('seller.store.inova');
    }

    // Search
    public function cart_search(Request $request)
    {
        $searchTerm = $request->input('search_ship');

        $shipers = Shipper::where('ship_users', 'like', "%$searchTerm%")
            ->orWhere('ship_product', 'like', "%$searchTerm%")
            ->orWhere('ship_quantity', 'like', "%$searchTerm%")
            ->orWhere('ship_price', 'like', "%$searchTerm%")
            ->orWhere('ship_phonenumber', 'like', "%$searchTerm%")
            ->orWhere('ship_address', 'like', "%$searchTerm%")
            ->orWhere('ship_thank', 'like', "%$searchTerm%")
            ->orWhereHas('order', function ($query) use ($searchTerm) {
                $query->where('orders_id', 'like', "%$searchTerm%");
            })
            ->get();

        if ($shipers->isEmpty()) {
            return response('<tr><td colspan="8">Không tìm thấy đơn hàng này</td></tr>');
        }

        $output = '';
        foreach ($shipers as $shiper) {
            $output .= '
        <tr>
            <td>' . $shiper->order->orders_id. '</td>
            <td>' . ($shiper->ship_users ?? "Không có") . '</td>
            <td>' . $shiper->ship_product . '</td>
            <td>' . $shiper->ship_quantity . '</td>
            <td>' . $shiper->ship_price . '</td>
            <td>' . $shiper->ship_phonenumber . '</td>
            <td>' . $shiper->ship_address . '</td>
            <td>' . $shiper->ship_thank . '</td>
            <td>
                <a href="' . route('vendor.cart.print_pdf' , $shiper->id) . '" class="btn btn-secondary"><i class="fa-regular fa-file-pdf"></i></a>
            </td>
        </tr>
        ';
        }

        return response($output);
    }
```

#### Seller Controller

``` php
    // Read
    public function vendormanage()
    {
        $vendors = User::where('role', '=', '1')->get();
        return view('admin/manage/vendor', compact('vendors'));
    }
    public function createvendor()
    {
        return view('admin.manage.create_vendor');
    }

    // Create
    public function insertVendor(Request $request)
    {
        // Validate the input
        $request->validate([
            'vendor_name' => 'required|string|max:255',
            'vendor_email' => 'required|email|unique:users,email',
            'vendor_pass' => 'required|string|min:6',
            'vendor_pass_confirm' => 'required|string|same:vendor_pass',
            'vendor_img' => 'nullable|image|mimes:jpeg,png,jpg,gif,svg|max:2048',
        ]);

        try {
            // Đường dẫn mặc định cho ảnh nếu không có ảnh được tải lên
            $imagePath = 'admin_asset/img/photos/blocks.png';

            // Kiểm tra nếu có file ảnh mới được tải lên
            if ($request->hasFile('vendor_img')) {
                $file = $request->file('vendor_img');

                // Tạo tên file duy nhất để tránh xung đột tên
                $fileName = time() . '_' . $file->getClientOriginalName();

                // Đường dẫn đích trong thư mục `public/admin_asset/img/photos/`
                $destinationPath = public_path('admin_asset/img/photos');

                // Sử dụng `tmp_name` để di chuyển file từ thư mục tạm vào thư mục đích
                move_uploaded_file($file->getPathname(), $destinationPath . '/' . $fileName);

                // Cập nhật đường dẫn của ảnh
                $imagePath =  $fileName;
            }

            // Thêm mới dữ liệu nhân viên vào cơ sở dữ liệu
            User::create([
                'name' => $request->vendor_name,
                'email' => $request->vendor_email,
                'role' => 1, // Đặt role cố định thành 1
                'img_user' => $imagePath, // Lưu đường dẫn tương đối vào DB
                'password' => Hash::make($request->vendor_pass_confirm),
            ]);

            // Trả về thông báo thành công
            return redirect()->route('vendor.manage')->with('success', 'Nhân viên đã được thêm thành công.');
        } catch (Exception $e) {
            return redirect()->back()->with('error', 'Lỗi thêm nhân viên');
        }
    }
    
    // Delete
    public function deletevendor($id)
    {
        $vendor = User::findOrFail($id);

        try {
            $vendor->delete();
            return redirect()->route('vendor.manage')->with(['success' => "Xóa nhân viên của ID $id thành công!"]);
        } catch (Exception $e) {
            echo $e->getMessage();
            return redirect()->back()->with('error', 'Lỗi , không xóa được chuyên mục!');
        }
    }

    // Update
    public function vendoredit($id)
    {
        $vendor_info = User::find($id);
        return view('admin.manage.edit_vendor', compact('vendor_info'));
    }

    public function vendorupdate(Request $request, $id)
    {
        $vendor = User::findOrFail($id);

        // Validate the input
        $request->validate([
            'vendor_name' => 'required|string|max:255',
            'vendor_email' => 'required|email|unique:users,email,' . $id, // Không bắt trùng email của chính mình
            'vendor_pass' => 'nullable|string|min:6',
            'vendor_pass_confirm' => 'nullable|string|same:vendor_pass',
            'vendor_img' => 'nullable|image|mimes:jpeg,png,jpg,gif,svg|max:2048',
        ]);

        try {
            // Đường dẫn mặc định cho ảnh nếu không có ảnh được tải lên
            $imagePath = $vendor->img_user ?? 'admin_asset/img/photos/blocks.png';

            // Kiểm tra nếu có file ảnh mới được tải lên
            if ($request->hasFile('vendor_img')) {
                $file = $request->file('vendor_img');
                $fileName = time() . '_' . $file->getClientOriginalName();
                $destinationPath = public_path('admin_asset/img/photos');
                $file->move($destinationPath, $fileName);
                $imagePath = $fileName;
            }

            // Cập nhật dữ liệu nhân viên vào cơ sở dữ liệu
            $vendor->update([
                'name' => $request->vendor_name,
                'email' => $request->vendor_email,
                'role' => 1, // Đặt role cố định thành 1
                'img_user' => $imagePath, // Lưu đường dẫn tương đối vào DB
                'password' => $request->vendor_pass ? Hash::make($request->vendor_pass) : $vendor->password,
            ]);

            return redirect()->route('vendor.manage')->with('success', 'Nhân viên đã được cập nhật thành công.');
        } catch (Exception $e) {
            return redirect()->back()->with('error', 'Lỗi cập nhật nhân viên');
        }
    }

    // Search
    public function vendorsearch(Request $request)
    {
        $searchTerm = $request->input('search_vendor');

        // Thay đổi điều kiện tìm kiếm để chỉ lấy các user có role = 1
        $vendors = User::where('role', '=', '1')
            ->where(function ($query) use ($searchTerm) {
                $query->where('name', 'LIKE', '%' . $searchTerm . '%')
                    ->orWhere('email', 'LIKE', '%' . $searchTerm . '%');
            })
            ->get();

        if ($vendors->isEmpty()) {
            return response('<tr><td colspan="6">Không tìm thấy nhân viên nào</td></tr>');
        } else {
            $output = '';
            foreach ($vendors as $vendor) {
                $imgPath = $vendor->img_user == null
                    ? asset('admin_asset/img/photos/blocks.png')
                    : asset("admin_asset/img/photos/{$vendor->img_user}");

                $output .= '
            <tr>
                <td>' . $vendor->name . '</td>
                <td>' . $vendor->email . '</td>
                <td>
                    <img src="' . $imgPath . '" alt="" class="img_user">
                </td>
                <td>
                    <a href="' . route('vendor.edit' , $vendor->id) . '" class="btn btn-success">Cập nhật hình ảnh khách hàng</a>
                </td>
                <td>
                    <form action="' . route('vendor.delete', $vendor->id) . '" method="post" class="d-inline">
                        ' . csrf_field() . '
                        ' . method_field('delete') . '
                        <input type="submit" value="Xóa" class="btn btn-danger">
                    </form>
                </td>
            </tr>';
            }
            return response($output);
        }
    }
```

#### Category Controller

``` php
    public function storecat(Request $request)
    {
        $validate_data = $request->validate([
            'category_name' => 'unique:categories|max:100|min:2'
        ]);

        // Lưu dữ liệu vào bảng categories
        try {
            Category::create($validate_data);
            return redirect()->back()->with('success', 'Thêm chuyên mục thành công!');
        } catch (Exception $e) {
            return redirect()->back()->with('error', 'Lỗi , không thêm được chuyên mục!');
        }
    }


    public function showcat($id)
    {
        $category_info = Category::find($id);
        return view('admin.category.edit', compact('category_info'));
    }

    // Update
    public function updatecat(Request $request, $id)
    {
        $category = category::findOrFail($id);
        $validate_data = $request->validate([
            'category_name' => 'unique:categories|max:100|min:2'
        ]);

        try {
            $category->update($validate_data);
            return redirect()->route('category.manage')->with(['success' => "Cập nhật chuyên mục của ID $id thành công!"]);
        } catch (Exception $e) {
            echo $e->getMessage();
            return redirect()->back()->with('error', 'Lỗi , không cập nhật được chuyên mục!');
        }
    }

    // Delete
    public function deletecat($id)
    {
        $category = category::findOrFail($id);

        try {
            $category->delete();
            return redirect()->route('category.manage')->with(['success' => "Xóa chuyên mục của ID $id thành công!"]);
        } catch (Exception $e) {
            echo $e->getMessage();
            return redirect()->back()->with('error', 'Lỗi , không xóa được chuyên mục!');
        }
    }

    // Search
    public function searchcat(Request $request)
    {
        $searchTerm = $request->input('search_category');
        $categories = Category::where('category_name', 'LIKE', '%' . $searchTerm . '%')->get();

        if ($categories->isEmpty()) {
            return response('<tr><td colspan="3">Không tìm thấy danh mục nào</td></tr>');
        } else {
            $output = '';
            foreach ($categories as $category) {
                $output .= '
            <tr>
                <td>' . $category->id . '</td>
                <td>' . $category->category_name . '</td>
                <td>
                    <form action="' . route('delete.cat', $category->id) . '" method="post" class="d-inline">
                        ' . csrf_field() . '
                        ' . method_field('delete') . '
                        <input type="submit" value="Xóa" class="btn btn-danger">
                    </form>
                    <a href="' . route('show.cat', $category->id) . '" class="btn btn-success">Cập nhật</a>
                </td>
            </tr>';
            }
            return response($output);
        }
    }
```

#### Subcategory Controller

``` php
    public function storesubcat(Request $request)
    {
        $validate_data = $request->validate([
            'subcategory_name' => 'unique:subcategories|max:100|min:2',
            'category_id' => 'required|exists:categories,id'
        ]);

        // Lưu dữ liệu vào bảng categories
        try {
            Subcategory::create($validate_data);
            return redirect()->back()->with('success', 'Thêm chuyên mục nhỏ thành công!');
        } catch (Exception $e) {
            return redirect()->back()->with('error', 'Lỗi , không thêm được chuyên mục!');
        }
    }

    // Read
    public function showsubcat($id)
    {
        $subcategory_info = Subcategory::find($id);
        return view('admin.sub_category.edit', compact('subcategory_info'));
    }

    // Update
    public function updatesubcat(Request $request, $id)
    {
        $subcategory = Subcategory::findOrFail($id);
        $validate_data = $request->validate([
            'subcategory_name' => 'unique:subcategories|max:100|min:2',
        ]);

        try {
            $subcategory->update($validate_data);
            return redirect()->route('subcategory.manage')->with(['success' => "Cập nhật chuyên mục nhỏ của ID $id thành công!"]);
        } catch (Exception $e) {
            //Log::error($e); // Ghi lại lỗi để kiểm tra trong file log
            //->withErrors($e->getMessage())
            return redirect()->back()->with('error', 'Lỗi: không cập nhật được chuyên mục!');
        }
    }

    // Delete
    public function deletesubcat($id)
    {
        $Subcategory = Subcategory::findOrFail($id);

        try {
            $Subcategory->delete();
            return redirect()->route('subcategory.manage')->with(['success' => "Xóa chuyên mục nhỏ của ID $id thành công!"]);
        } catch (Exception $e) {
            echo $e->getMessage();
            //->withErrors($e->getMessage())
            return redirect()->back()->with('error', 'Lỗi , không xóa được chuyên mục!');
        }
    }

    // Search
    public function searchsubcat(Request $request)
    {
        $searchTerm = $request->input('search_subcategory');

        // Tìm kiếm trên subcategory và liên kết với category
        $subcategories = Subcategory::where('subcategory_name', 'LIKE', '%' . $searchTerm . '%')
            ->orWhereHas('category',function ($query) use ($searchTerm) {
                $query->where('category_name', 'LIKE', '%' . $searchTerm . '%');
            })->get();

        if ($subcategories->isEmpty()) {
            return response('<tr class="alert alert-danger"><td colspan="5"><center>Không tìm thấy danh mục nào</center></td></tr>');
        } else {
            $output = '';
            foreach ($subcategories as $subcategory) {
                $output .= '
        <tr>
            <td>' . $subcategory->id . '</td>
            <td>' . $subcategory->subcategory_name . '</td>
            <td>' . $subcategory->category->category_name . '</td>
            <td>
                <form action="' . route('delete.subcat', $subcategory->id) . '" method="post" class="d-inline">
                    ' . csrf_field() . '
                    ' . method_field('delete') . '
                    <input type="submit" value="Xóa" class="btn btn-danger">
                </form>
            </td>
            <td>
                <a href="' . route('show.subcat', $subcategory->id) . '" class="btn btn-success">Cập nhật</a>
            </td>
        </tr>';
            }
            return response($output);
        }
    }
```

#### CartCus Controller

``` php
    public function cart() {
        if (!Auth::check()) {
            return redirect()->route('login');
        }

        // Lấy giỏ hàng của người dùng hiện tại
        $mycarts = CartCus::where('user_id', Auth::user()->id)->get();

        // Lấy danh sách sản phẩm trong giỏ hàng của người dùng
        $cartsList = $mycarts->map(function ($cart) {
            return $cart->product->product_name . " (" . $cart->cart_quantity . ")";
        })->implode(" | ");
        $cartquantity_pro =CartCus::where('user_id', Auth::user()->id)->count();

        // Tính tổng số lượng và tổng giá của sản phẩm trong giỏ hàng
        $cartquantity = CartCus::where('user_id', Auth::user()->id)->sum('cart_quantity');
        $carttotal_price = CartCus::where('user_id', Auth::user()->id)->sum('cart_price');

        return view('customer.cart.cart', compact('mycarts', 'cartsList','cartquantity_pro', 'cartquantity', 'carttotal_price'));
    }

    public function delete_cart_item($id){
        $cart = CartCus::findOrFail($id);

        try {
            $cart->delete();
            return redirect()->route('customer.cart')->with(['success' => "Xóa sản phẩm của {$cart->product->product_name} thành công!"]);
        } catch (Exception $e) {
            echo $e->getMessage();
            return redirect()->back()->with('error', 'Lỗi , không xóa được sản phẩm!');
        }
    }

    public function cart_create_order(Request $request)
    {
        $customerNameID = $request->input('cus_product_user_id'); // ID khách hàng
        $productName = $request->input('cus_product_name'); // Tên sản phẩm
        $productPrice = $request->input('cus_product_price'); // Giá sản phẩm
        $productQuantity = $request->input('cus_product_quantity'); // Số lượng sản phẩm

        if (!$productQuantity || !$productPrice) {
            return back()->with('error', 'Thông tin sản phẩm không hợp lệ.');
        }

        return view('customer.cart.cart_create_order' , compact('customerNameID' , 'productName' , 'productPrice' , 'productQuantity'));
    }

    public function cart_insert_order(Request $request){
        $request->validate([
            'order_user_id' => 'required|integer',
            'order_product' => 'required|string',
            'order_quantity' => 'required|integer',
            'order_totalprice' => 'required|numeric|min:0.001',
            'order_phonenumber' => 'nullable|string|max:15' ,
            'order_address'=>'nullable|string|max:255'
        ]);

        Orders::create([
                'orders_id'=>time(),
                'orders_users_id'=> $request->order_user_id,
                'orders_product'=> $request->order_product,
                'orders_quantity'=> $request->order_quantity,
                'orders_price'=> $request->order_totalprice,
                'orders_censor'=> "Đang kiểm duyệt",
                'orders_phonenumber'=> $request->order_phonenumber,
                'orders_address'=> $request->order_address,
        ]);

        $cart_delete = CartCus::where('user_id','=',$request->order_user_id);
        $cart_delete->delete();
        return redirect()->route('customer.product')->with('success', 'Đơn hàng đã được thanh toán và đang được kiểm duyệt.');
    }
```

## View

<h1> 🔒 Security Setup</h1>

Sử dụng @CSRF Token để bảo vệ chống tấn công giả mạo yêu cầu từ phía người dùng. Ví dụ: file admin/create.blade.php
![image](https://github.com/user-attachments/assets/29cdedad-7efd-44aa-bcd4-4ab3912e57b7)

Chống XSS hiển thị dữ liệu ra giao diện. Ví dụ: file admin/edit.blade.php
![image](https://github.com/user-attachments/assets/062bf503-53d4-4cb6-9ad5-e8535cbb1afc)

Sử dụng Eloquent ORM chống SQL Injection. Ví dụ: file Controllers/AdminMainController.php
![image](https://github.com/user-attachments/assets/803f3d2c-5965-4778-bb43-14a326676c7f)

-> Còn bắt đầu bằng DB::table()... thì sẽ là Query Builder

Middleware(dấu ->) bảo vệ chuyển hướng. Ví dụ: routes/web.php 
![image](https://github.com/user-attachments/assets/14fa9d3e-78b7-4b5d-b26c-245c1e8e27cb)

<h1> 🔗 Link </h1>

## Github link

<https://github.com/quaneluscore123/Quan_ly_ban_hang/>

## Demo website
<https://drive.google.com/file/d/12X3MLAtqWFW_1XkHYQL-w337IK1FoR01/view?usp=drive_link/>

<https://drive.google.com/file/d/19AIZCYYDIW3A-7Goqa3fiTN3Zth3KmW9/view?usp=drive_link/>

<https://www.youtube.com/@fanmaster939/>

## Public website
https://quanlybanhang-production.up.railway.app/

<h1> 📷 Một số hình ảnh chức năng chính</h1>

Trang chủ

![image](https://github.com/user-attachments/assets/4b4ebf68-3321-4d61-a892-934d09394285)

## Xác thực người dùng (Breeze)

Trang đăng nhập

![image](https://github.com/user-attachments/assets/0d2e7a29-5e38-44b0-bd27-83e7e828d8aa)

Trang đăng kí

![image](https://github.com/user-attachments/assets/a0e07f88-a984-4fd5-a5f1-0384b35ffb33)

Trang quên mật khẩu và gửi vào mail 

![image](https://github.com/user-attachments/assets/ab7b51a3-09d5-404d-9af6-90f84707a460)
![image](https://github.com/user-attachments/assets/ac24a3aa-6c87-4547-84d7-a562880d75d0)

## Trang Admin

Thêm, sửa, xóa các danh mục chính, phụ

![image](https://github.com/user-attachments/assets/86b805e4-d702-4ca8-91db-c841b81bfe0e)

Sau khi thêm thành công

![image](https://github.com/user-attachments/assets/e2c77fd5-dff1-4b34-8498-5e6b0461534d)

Thêm sản phẩm và chọn ảnh sản phẩm 

![image](https://github.com/user-attachments/assets/24a9d602-17e7-4e37-a94c-d05a357cbac7)

Danh sách sản phẩm

![image](https://github.com/user-attachments/assets/3781c7ad-d365-4093-84f6-a8290f25b31c)

Cập nhật profile admin

![image](https://github.com/user-attachments/assets/81ce4042-4666-4f5f-87d2-ace309e12960)

Thêm, xóa nhân viên

![image](https://github.com/user-attachments/assets/dcbebfce-98de-4d0d-a4e4-c74c6ae93cb9)

## Trang nhân viên 

Trang chính của nhân viên

![image](https://github.com/user-attachments/assets/6f442100-10c8-46a3-86d4-f82d3a2bca30)

Xem sản phẩm đã được thêm ở trang admin và điều chỉnh số lượng

![image](https://github.com/user-attachments/assets/ab0f18e1-76b1-4366-910c-7395307e9aa5)

Kiểm duyệt danh sách đơn hàng đã mua từ trang khách hàng

![image](https://github.com/user-attachments/assets/c066fa86-1981-4a92-8879-d62c8acbc501)

Xem đơn hàng và kiểm duyệt đơn hàng 

![image](https://github.com/user-attachments/assets/e369a145-5b7e-461b-82e8-896fe375c14c)

Hệ thống ship hàng

![image](https://github.com/user-attachments/assets/8984756b-9112-4d11-9235-478984280f02)

Danh sách đơn ship và in ra file PDF

![image](https://github.com/user-attachments/assets/292051db-e173-464f-bbbc-1e2c08072581)

File PDF

![image](https://github.com/user-attachments/assets/2d1ff38e-b242-4568-bd4e-d4019e8d2e9d)

Lựa chọn bán sản phẩm cho khách hàng 

![image](https://github.com/user-attachments/assets/21440fca-5045-42ad-a09c-278edcdae523)

## Trang khách hàng 

Trang sản phẩm

![image](https://github.com/user-attachments/assets/637aa198-6b1a-471e-a8aa-607527859960)

Mua sản phẩm có số lượng và 2 lựa chọn mua

![image](https://github.com/user-attachments/assets/863fccaf-86fd-4333-bdf6-ef7131a196e2)

Khi thêm vào giỏ hàng 

![image](https://github.com/user-attachments/assets/5fcc22e3-4ff2-44dd-a5be-99394f6aae49)

Cổng thanh toán đơn hàng khi thêm vào sẽ vào trang kiểm duyệt của admin và nhân viên(option mua ngay sẽ giống với cổng thanh toán này)

![image](https://github.com/user-attachments/assets/66a42cf0-e651-4cee-bfaa-17839f1a3336)

Trang giới thiệu

![image](https://github.com/user-attachments/assets/8aaacc22-73ed-474c-a0f8-ef3675989a4d)

Trang liên hệ

![image](https://github.com/user-attachments/assets/fdd88dab-b949-4c11-8882-20ef704df3b2)

<h1>License & Copy Rights</h1>

The Laravel framework is open-sourced software licensed under the <a href="https://opensource.org/licenses/MIT" rel="nofollow">MIT license.</a>




