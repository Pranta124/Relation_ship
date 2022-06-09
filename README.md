# Relation_ship
# One_To_One Controller
**

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

**
