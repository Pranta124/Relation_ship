# Relation_ship
# Controller_Method
```
One_To_One
<?php

namespace App\Http\Controllers;

use App\Models\Profile;
use App\Models\User;
use Illuminate\Http\Request;

class OneToOneController extends Controller
{
    public function index()
    {
        // return $user = User::find(10)->profile()->create([
        //     'city'=>'dhaka',
        //     'zip_code'=>'1203',
        //     'view'=>10,
        //     'phone'=>012102541352
        // ]);
        //  $users=User::with('profile')->get();//Eger load;
        //  $users = User::get();
        //  $users->load('profile');/lazy load;
        // $users = User::whereHas('profile')->get();
        // $users = User::whereHas('profile',function($q)
        // {
        //     $q->whereNotNull('zip_code');
        // })->get();
        // $users = User::whereHas('profile',function($q)
        // {
        //     $q->whereNull('zip_code');
        // })->get();
            $profiles = Profile::get();
        return view('one_to_one',compact('profiles'));
    }
}

```
```
One_TO_Many
<?php

namespace App\Http\Controllers;

use App\Models\Category;
use Illuminate\Http\Request;

class OneToManyController extends Controller
{
    public function index()
    {
        // return $categories = Category::whereHas('products',function($q){
        //     $q->whereNotNull('category_id');
        // })->get();
        // Category::withOnly(['products'])->get();
        // Category::without('products')->get();
        return $categories = Category::with('products.category')->get();
        // return $categories = Category::with('products.category')->get();

        return view('one_to_many');
    }
    public function category(Category $id)
    {
        return $id->load('latest_product');
    }
}

```
```
Has_One_through
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Models\Machanic;
use App\Models\Car;
use App\Models\Owner;
class Has_One_ThroughController extends Controller
{
    public function index()
    {
        $mechanic = Machanic::with('owner')->get();
        return $mechanic;
    }
}

```
# Models
```
User_Models
<?php

namespace App\Models;

use Illuminate\Contracts\Auth\MustVerifyEmail;
use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Notifications\Notifiable;
use Laravel\Sanctum\HasApiTokens;

class User extends Authenticatable
{
    use HasApiTokens, HasFactory, Notifiable;

    /**
     * The attributes that are mass assignable.
     *
     * @var array<int, string>
     */
    protected $fillable = [
        'name',
        'email',
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
     * The attributes that should be cast.
     *
     * @var array<string, string>
     */
    protected $casts = [
        'email_verified_at' => 'datetime',
    ];
    public function profile()
    {
        return $this->hasOne(Profile::class,'user_id','id');
    }
}

```
```
Profile_Php
<?php

namespace App\Models;
use App\Models\User;
use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Profile extends Model
{
    use HasFactory;
    protected $guarded = [];
    public function user()
    {
        return $this->belongsTo(User::class,'user_id','id');
    }
}


```
```
Product_php
<?php

namespace App\Models;
use App\Models\User;
use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Profile extends Model
{
    use HasFactory;
    protected $guarded = [];
    public function user()
    {
        return $this->belongsTo(User::class,'user_id','id');
    }
}


```
```
Owner.php
```
```
Mechanic.php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Relations\HasOneThrough;

class Machanic extends Model
{
    use HasFactory;
    protected $guarded = [];
     public function owner():HasOneThrough
     {
         return $this->hasOneThrough(Owner::class,Car::class,'mechanic_id','car_id');
     }
}

```
```
Category_php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Relations\HasMany;

class Category extends Model
{
    use HasFactory;
    //
    protected $guarded = [];
    public function products():HasMany //type casting
    {
        return $this->hasMany(Product::class,'category_id','id');
    }
    public function latest_product()
    {
        return $this->hasOne(Product::class)->latestOfMany();
    }
}

```
```
Car.php
```
## web.php
```
<?php

use App\Http\Controllers\OneToManyController;
use App\Http\Controllers\OneToOneController;
use App\Http\Controllers\Has_One_ThroughController;
use App\Models\Machanic;
use Illuminate\Database\Eloquent\Relations\HasOneThrough;
use Illuminate\Support\Facades\Route;
/*
|--------------------------------------------------------------------------
| Web Routes
|--------------------------------------------------------------------------
|
| Here is where you can register web routes for your application. These
| routes are loaded by the RouteServiceProvider within a group which
| contains the "web" middleware group. Now create something great!
|
*/

Route::get('/', function () {
    return view('welcome');
});
Route::get('one_to_one',[OneToOneController::class,'index']);
Route::get('has_one_throuch',[Has_One_ThroughController::class,'index']);
Route::get('one_to_many',[OneToManyController::class,'index']);
 Route::get('category/{id}',[OneToManyController::class,'category']);

```
