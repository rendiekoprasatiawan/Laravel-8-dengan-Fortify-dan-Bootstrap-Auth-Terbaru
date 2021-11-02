# Laravel-8-dengan-Fortify-dan-Bootstrap-Auth-Terbaru

Dokumentasi laravel auth menggunakan laravel fortify dan frameworks css bootstrap sebagai user interface dalam penerapannya

<hr>

* Install laravel dengan nama-project yang akan anda gunakan

  > composer create-project --prefer-dist laravel/laravel nama-project

* Buat nama database pada web server yang anda gunakan

  > http://localhost/phpmyadmin

* Pada file .env dalam project laravel, konfigurasi nama database yang sudah anda buat
  ```
  DB_CONNECTION=mysql
  DB_HOST=127.0.0.1
  DB_PORT=3306
  DB_DATABASE=nama-database-kalian
  DB_USERNAME=root
  DB_PASSWORD=
  ```

* Install package manager fortify terlebih dahulu
   
  > composer require laravel/fortify

* Ketika laravel fortify sudah terinstall, selanjutnya publish

  > php artisan vendor:publish --provider="Laravel\Fortify\FortifyServiceProvider"

* Migrasi database

  > php artisan migrate

* Install bootstrap, jquery, dan popper
   
  > npm i
  
  > npm install --save bootstrap jquery popper.js cross-env

* Import packages
  > resources/js/bootstrap.js
  
  ```
  try {
      window.Popper = require("popper.js").default;
      window.$ = window.jQuery = require("jquery");

      require("bootstrap");
  } catch (e) {}
  ```

* Hapus folder css
  > resources/css
  ```
  rm -rf resources/css
  ```

* Buat folder sass
  > resources/sass
  ```
  mkdir resources/sass
  ```

* Buat file app.scss
  > resources/sass/app.scss
  ```
  touch resources/sass/app.scss
  ```

* Import packages pada
  > resources/sass/app.scss
  ```
  // bootstrap
  @import "~bootstrap/scss/bootstrap";
  ```

* Update webpack
  > webpack.mix.js
  ```
  mix.js('resources/js/app.js', 'public/js')
      .sass('resources/sass/app.scss', 'public/css');
  ```

* Compile assets
  ```  
  npm run dev
  ```

* Ketika ada issue run mix.js again, ini adalah problem solvernya
  ```
  npm install && npm run dev
  ```

* Tambahkan folder auth dan layouts di views
  > resources/views/auth
  
  > resources/views/layouts
  ```
  mkdir resources/views/auth resources/views/layouts
  ```

* Buat file app.blade.php pada
  > resources/views/layouts
  ```
  touch resources/views/layouts/app.blade.php
  ```
  
* Buat file login.blade.php pada
  > resources/views/auth
  ```
  touch resources/views/auth/login.blade.php
  ```

* Buat file register.blade.php pada
  > resources/views/auth
  ```
  touch resources/views/auth/register.blade.php
  ```

* Buat file forgot-password.blade.php pada
  > resources/views/auth
  ```
  touch resources/views/auth/forgot-password.blade.php
  ```

* Buat file reset-password.blade.php pada
  > resources/views/auth
  ```
  touch resources/views/auth/reset-password.blade.php
  ```

* Buat file verify-email.blade.php pada
  > resources/views/auth
  ```
  touch resources/views/auth/verify-email.blade.php
  ```

* Buat file home.blade.php pada
  > resources/views
  ```
  touch resources/views/home.blade.php
  ```

* Pada file app.blade.php salin ini
  ```
  <!DOCTYPE html>
  <html lang="{{ str_replace('_', '-', app()->getLocale()) }}">
    <head>
      <meta charset="utf-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1" />

      <!-- CSRF Token -->
      <meta name="csrf-token" content="{{ csrf_token() }}" />

      <title>{{ config('app.name', 'Laravel') }}</title>

      <!-- Scripts -->
      <script src="{{ asset('js/app.js') }}" defer></script>

      <!-- Fonts -->
      <link rel="dns-prefetch" href="//fonts.gstatic.com" />
      <link
        href="https://fonts.googleapis.com/css2?family=Nunito+Sans:wght@400;600&display=swap"
        rel="stylesheet"
      />

      <!-- Styles -->
      <link href="{{ asset('css/app.css') }}" rel="stylesheet" />
    </head>
    <body>
      <div id="app">
        <nav class="navbar navbar-expand-md navbar-dark bg-primary">
          <div class="container">
            <a class="navbar-brand" href="{{ url('/') }}">
              {{ config('app.name', 'Laravel') }}
            </a>
            <button
              class="navbar-toggler"
              type="button"
              data-toggle="collapse"
              data-target="#navbarSupportedContent"
              aria-controls="navbarSupportedContent"
              aria-expanded="false"
              aria-label="{{ __('Toggle navigation') }}"
            >
              <span class="navbar-toggler-icon"></span>
            </button>

            <div class="collapse navbar-collapse" id="navbarSupportedContent">
              <!-- Left Side Of Navbar -->
              <ul class="navbar-nav mr-auto"></ul>

              <!-- Right Side Of Navbar -->
              <ul class="navbar-nav ml-auto">
                <!-- Authentication Links -->
                @guest
                <li class="nav-item">
                  <a class="nav-link" href="{{ route('login') }}"
                    >{{ __('Login') }}</a
                  >
                </li>
                @if (Route::has('register'))
                <li class="nav-item">
                  <a class="nav-link" href="{{ route('register') }}"
                    >{{ __('Register') }}</a
                  >
                </li>
                @endif @else
                <li class="nav-item dropdown">
                  <a
                    id="navbarDropdown"
                    class="nav-link dropdown-toggle"
                    href="#"
                    role="button"
                    data-toggle="dropdown"
                    aria-haspopup="true"
                    aria-expanded="false"
                    v-pre
                  >
                    {{ Auth::user()->name }}
                  </a>

                  <div
                    class="dropdown-menu dropdown-menu-right"
                    aria-labelledby="navbarDropdown"
                  >
                    <a
                      class="dropdown-item"
                      href="{{ route('logout') }}"
                      onclick="event.preventDefault();
                                                       document.getElementById('logout-form').submit();"
                    >
                      {{ __('Logout') }}
                    </a>

                    <form
                      id="logout-form"
                      action="{{ route('logout') }}"
                      method="POST"
                      class="d-none"
                    >
                      @csrf
                    </form>
                  </div>
                </li>
                @endguest
              </ul>
            </div>
          </div>
        </nav>

        <main class="py-4">@yield('content')</main>
      </div>
    </body>
  </html>
  ```
  
* Pada file forgot-password.blade.php salin ini
  ``` 
  @extends('layouts.app') @section('content')
  <div class="container">
    <div class="row justify-content-center">
      <div class="col-md-8">
        <div class="card">
          <div class="card-header">{{ __('Reset Password') }}</div>

          <div class="card-body">
            @if (session('status'))
            <div class="alert alert-success" role="alert">
              {{ session('status') }}
            </div>
            @endif

            <form method="POST" action="{{ route('password.email') }}">
              @csrf

              <div class="form-group row">
                <label for="email" class="col-md-4 col-form-label text-md-right"
                  >{{ __('E-Mail Address') }}</label
                >

                <div class="col-md-6">
                  <input
                    id="email"
                    type="email"
                    class="form-control @error('email') is-invalid @enderror"
                    name="email"
                    value="{{ old('email') }}"
                    required
                    autocomplete="email"
                    autofocus
                  />

                  @error('email')
                  <span class="invalid-feedback" role="alert">
                    <strong>{{ $message }}</strong>
                  </span>
                  @enderror
                </div>
              </div>

              <div class="form-group row mb-0">
                <div class="col-md-6 offset-md-4">
                  <button type="submit" class="btn btn-primary">
                    {{ __('Send Password Reset Link') }}
                  </button>
                </div>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
  @endsection
  ```
  
* Pada file home.blade.php salin ini
  ```
  @extends('layouts.app') @section('content')
  <div class="container">
    @if (session('status'))
    <div class="alert alert-success" role="alert">{{ session('status') }}</div>
    @endif

    <div class="jumbotron">
      <h5>Welcome, {{ auth()->user()->email }}</h5>
      <h1 class="display-3">Bootstrap 4 Laravel Fortify Authentication</h1>
      <p class="lead">
        This is a simple auth starter setup for laravel 8 projects
      </p>
      <hr class="my-4" />
      <h2>Features:</h2>
      <ul>
        <li>User Login</li>
        <li>User Registration</li>
        <li>Email Verification</li>
        <li>Forget Password</li>
        <li>Reset Password</li>
      </ul>
      <p class="lead">
        <a class="btn btn-primary" href="" target="_blank" role="button"
          >Github Source Code</a
        >
      </p>
    </div>
  </div>
  @endsection
  ```

* Pada file login.blade.php salin ini
  ```
  @extends('layouts.app')

  @section('content')
      <div class="container">
          <div class="row justify-content-center">
              <div class="col-md-8">
                  <div class="card">
                      <div class="card-header">{{ __('Login') }}</div>

                      <div class="card-body">
                          <form method="POST" action="{{ route('login') }}">
                              @csrf

                              <div class="form-group row">
                                  <label for="email"
                                         class="col-md-4 col-form-label text-md-right">{{ __('E-Mail Address') }}</label>

                                  <div class="col-md-6">
                                      <input id="email" type="email"
                                             class="form-control @error('email') is-invalid @enderror" name="email"
                                             value="{{ old('email') }}" required autocomplete="email" autofocus>

                                      @error('email')
                                      <span class="invalid-feedback" role="alert">
                                          <strong>{{ $message }}</strong>
                                      </span>
                                      @enderror
                                  </div>
                              </div>

                              <div class="form-group row">
                                  <label for="password"
                                         class="col-md-4 col-form-label text-md-right">{{ __('Password') }}</label>

                                  <div class="col-md-6">
                                      <input id="password" type="password"
                                             class="form-control @error('password') is-invalid @enderror" name="password"
                                             required autocomplete="current-password">

                                      @error('password')
                                      <span class="invalid-feedback" role="alert">
                                          <strong>{{ $message }}</strong>
                                      </span>
                                      @enderror
                                  </div>
                              </div>

                              <div class="form-group row">
                                  <div class="col-md-6 offset-md-4">
                                      <div class="form-check">
                                          <input class="form-check-input" type="checkbox" name="remember"
                                                 id="remember" {{ old('remember') ? 'checked' : '' }}>

                                          <label class="form-check-label" for="remember">
                                              {{ __('Remember Me') }}
                                          </label>
                                      </div>
                                  </div>
                              </div>

                              <div class="form-group row mb-0">
                                  <div class="col-md-8 offset-md-4">
                                      <button type="submit" class="btn btn-primary">
                                          {{ __('Login') }}
                                      </button>

                                      @if (Route::has('password.request'))
                                          <a class="btn btn-link" href="{{ route('password.request') }}">
                                              {{ __('Forgot Your Password?') }}
                                          </a>
                                      @endif
                                  </div>
                              </div>
                          </form>
                      </div>
                  </div>
              </div>
          </div>
      </div>
  @endsection
  ```

* Pada file register.blade.php salin ini
  ```
  @extends('layouts.app') @section('content')
  <div class="container">
    <div class="row justify-content-center">
      <div class="col-md-8">
        <div class="card">
          <div class="card-header">{{ __('Register') }}</div>

          <div class="card-body">
            <form method="POST" action="{{ route('register') }}">
              @csrf

              <div class="form-group row">
                <label for="name" class="col-md-4 col-form-label text-md-right"
                  >{{ __('Name') }}</label
                >

                <div class="col-md-6">
                  <input
                    id="name"
                    type="text"
                    class="form-control @error('name') is-invalid @enderror"
                    name="name"
                    value="{{ old('name') }}"
                    required
                    autocomplete="name"
                    autofocus
                  />

                  @error('name')
                  <span class="invalid-feedback" role="alert">
                    <strong>{{ $message }}</strong>
                  </span>
                  @enderror
                </div>
              </div>

              <div class="form-group row">
                <label for="email" class="col-md-4 col-form-label text-md-right"
                  >{{ __('E-Mail Address') }}</label
                >

                <div class="col-md-6">
                  <input
                    id="email"
                    type="email"
                    class="form-control @error('email') is-invalid @enderror"
                    name="email"
                    value="{{ old('email') }}"
                    required
                    autocomplete="email"
                  />

                  @error('email')
                  <span class="invalid-feedback" role="alert">
                    <strong>{{ $message }}</strong>
                  </span>
                  @enderror
                </div>
              </div>

              <div class="form-group row">
                <label
                  for="password"
                  class="col-md-4 col-form-label text-md-right"
                  >{{ __('Password') }}</label
                >

                <div class="col-md-6">
                  <input
                    id="password"
                    type="password"
                    class="form-control @error('password') is-invalid @enderror"
                    name="password"
                    required
                    autocomplete="new-password"
                  />

                  @error('password')
                  <span class="invalid-feedback" role="alert">
                    <strong>{{ $message }}</strong>
                  </span>
                  @enderror
                </div>
              </div>

              <div class="form-group row">
                <label
                  for="password-confirm"
                  class="col-md-4 col-form-label text-md-right"
                  >{{ __('Confirm Password') }}</label
                >

                <div class="col-md-6">
                  <input
                    id="password-confirm"
                    type="password"
                    class="form-control"
                    name="password_confirmation"
                    required
                    autocomplete="new-password"
                  />
                </div>
              </div>

              <div class="form-group row mb-0">
                <div class="col-md-6 offset-md-4">
                  <button type="submit" class="btn btn-primary">
                    {{ __('Register') }}
                  </button>
                </div>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
  @endsection
  ```

* Pada file reset-password.blade.php salin ini
  ```
  @extends('layouts.app') @section('content')
  <div class="container">
    <div class="row justify-content-center">
      <div class="col-md-8">
        <div class="card">
          <div class="card-header">{{ __('Reset Password') }}</div>

          <div class="card-body">
            <form method="POST" action="{{ route('password.update') }}">
              @csrf

              <input type="hidden" name="token" value="{{ $request->route('token') }}" />

              <div class="form-group row">
                <label for="email" class="col-md-4 col-form-label text-md-right"
                  >{{ __('E-Mail Address') }}</label
                >

                <div class="col-md-6">
                  <input
                    id="email"
                    type="email"
                    class="form-control @error('email') is-invalid @enderror"
                    name="email"
                    value="{{ $email ?? old('email') }}"
                    required
                    autocomplete="email"
                    autofocus
                  />

                  @error('email')
                  <span class="invalid-feedback" role="alert">
                    <strong>{{ $message }}</strong>
                  </span>
                  @enderror
                </div>
              </div>

              <div class="form-group row">
                <label
                  for="password"
                  class="col-md-4 col-form-label text-md-right"
                  >{{ __('Password') }}</label
                >

                <div class="col-md-6">
                  <input
                    id="password"
                    type="password"
                    class="form-control @error('password') is-invalid @enderror"
                    name="password"
                    required
                    autocomplete="new-password"
                  />

                  @error('password')
                  <span class="invalid-feedback" role="alert">
                    <strong>{{ $message }}</strong>
                  </span>
                  @enderror
                </div>
              </div>

              <div class="form-group row">
                <label
                  for="password-confirm"
                  class="col-md-4 col-form-label text-md-right"
                  >{{ __('Confirm Password') }}</label
                >

                <div class="col-md-6">
                  <input
                    id="password-confirm"
                    type="password"
                    class="form-control"
                    name="password_confirmation"
                    required
                    autocomplete="new-password"
                  />
                </div>
              </div>

              <div class="form-group row mb-0">
                <div class="col-md-6 offset-md-4">
                  <button type="submit" class="btn btn-primary">
                    {{ __('Reset Password') }}
                  </button>
                </div>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
  @endsection
  ```

* Pada file verify-email.blade.php salin ini
  ```
  @extends('layouts.app') @section('content')
  <div class="container">
    <div class="row justify-content-center">
      <div class="col-md-8">
        <div class="card">
          <div class="card-header">{{ __('Verify Your Email Address') }}</div>

          <div class="card-body">
            @if (session('resent'))
            <div class="alert alert-success" role="alert">
              {{ __('A fresh verification link has been sent to your email
              address.') }}
            </div>
            @endif {{ __('Before proceeding, please check your email for a
            verification link.') }} {{ __('If you did not receive the email') }},
            <form
              class="d-inline"
              method="POST"
              action="{{ route('verification.send') }}"
            >
              @csrf
              <button type="submit" class="btn btn-link p-0 m-0 align-baseline">
                {{ __('click here to request another') }}
              </button>
              .
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
  @endsection
  ```

* Update Fortify Provider pada
  > app/Providers/FortifyServiceProvider :
  ``` 
  public function boot()
  {
      Fortify::loginView(function () {
          return view('auth.login');
      });

      Fortify::authenticateUsing(function (Request $request) {
          $user = User::where('email', $request->email)->first();

          if ($user &&
              Hash::check($request->password, $user->password)) {
              return $user;
          }
      });

      Fortify::registerView(function () {
          return view('auth.register');
      });

      Fortify::requestPasswordResetLinkView(function () {
          return view('auth.forgot-password');
      });

      Fortify::resetPasswordView(function ($request) {
          return view('auth.reset-password', ['request' => $request]);
      });

      Fortify::verifyEmailView(function () {
          return view('auth.verify-email');
      });

      // ...
  }
  ```

* Registerkan FortifyServiceProvide dengan menambahkannya pada
  > config\app.php
  ```
  App\Providers\FortifyServiceProvider::class,
  ```

* Implementasikan MustVerifyEmail pada file
  > app/Models/User.php
  ```
  <?php

  namespace App\Models;

  use Illuminate\Contracts\Auth\MustVerifyEmail;
  use Illuminate\Database\Eloquent\Factories\HasFactory;
  use Illuminate\Foundation\Auth\User as Authenticatable;
  use Illuminate\Notifications\Notifiable;

  class User extends Authenticatable implements MustVerifyEmail
  {
      use HasFactory, Notifiable;

      // ...
  }
  ```

* Pada file app/portify.php uncomment ini :
  ```
  Features::emailVerification()
  ```

* Dalam menggunakan email testing, harus mendaftar pada
  > https://mailtrap.io/

* Pada file .env, konfigurasi sesuai dengan data yang digunakan pada mailtrap.io
  ```
  MAIL_MAILER=smtp
  MAIL_HOST=smtp.mailtrap.io
  MAIL_PORT=2525
  MAIL_USERNAME=inipunyasaya
  MAIL_PASSWORD=inipunyasaya
  MAIL_ENCRYPTION=tls
  MAIL_FROM_ADDRESS=iniemaildummybebasaja@gmail.com
  MAIL_FROM_NAME="${APP_NAME}"
  ```

* Pada file routes/web.php tambahkan ini
  ```
  Route::middleware(['auth', 'verified'])->group(function () {
      Route::view('home', 'home')->name('home');
  });
  ``` 

* Terakhir, jalankan artisan
  ```
  php artisan serve
  ```
