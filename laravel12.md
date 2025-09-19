---
theme: jekyll-theme-dinky
---
# Laravel 12
### ¿Que es un Framework?
Un framework de desarrollo web es un conjunto de herramientas, bibliotecas y estructuras prediseñadas que facilitan y agilizan la creación de aplicaciones o sitios web. 
Proporciona una base organizada para desarrollar proyectos, permitiendo a los desarrolladores enfocarse en la lógica y funcionalidades específicas sin tener que construir desde cero elementos comunes como acceso a bases de datos, manejo de sesiones, validación de formularios, y generación de interfaces.

### ¿Que es Laravel?
Laravel es un framework de aplicaciones web con una sintaxis expresiva y elegante. Un framework web proporciona una estructura y un punto de partida para crear tu aplicación, lo que te permite centrarte en crear algo increíble mientras nosotros nos ocupamos de los detalles. 

#### Stack de herramientas 
- XAMPP (Apache, PHP, MySQL)
- Composer
- Visual Studio Code

#### Requisistos para Laravel 12 
- PHP >= 8.2

#### Crear proyecto

$ composer global require laravel/installer
$ laravel new example-app

#### Archivo de configuración
En la raíz del proyecto en el archivo .env

#### Validación Español
$ composer require laravel-lang/common
$ php artisan lang:add es
En tu archivo config/app.php, verifica que tengas esta linea:
'locale'=>'es'

### Layout
#### layouts/app.blade.php
```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Mi empresa - @yield('title')</title>
    <link href="{{ url('/css/bootstrap.min.css'); }}" rel="stylesheet" >
  </head>
  <body>
    <header>
        <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
            <div class="container-fluid">
                <a class="navbar-brand" href="#">Mi Empresa</a>
                <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
                    <span class="navbar-toggler-icon"></span>
                </button>
                <div class="collapse navbar-collapse" id="navbarNav">
                    <ul class="navbar-nav me-auto mb-2 mb-lg-0">
                        <li class="nav-item">
                            <a class="nav-link" href="{{ url('/') }}">Inicio</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="{{ url('/producto') }}">Productos</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="{{ url('/busqueda') }}">Búsqueda</a>
                        </li>
                    </ul>
                </div>
            </div>
        </nav>
    </header>

    <main>
        <div class="container-fluid">
            <div class="row">
                <div class="col-12">
                     @yield('content')
                </div>
            </div>
        </div>
    </main>

    <footer class="text-center py-3 mt-auto">
        <div class="container-fluid">
            <p>&copy; 2025 Mi Empresa. Todos los derechos reservados.</p>
        </div>
    </footer>
    
    <script src="{{ url('js/bootstrap.bundle.min.js'); }}"></script>
  </body>
</html>
```
### Modelo

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Producto extends Model
{
    protected $primaryKey = 'id';
    public $timestamps = false;
    protected $table = 'productos';
}

```
### Controller

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use Illuminate\View\View;
use Illuminate\Http\JsonResponse;

class EjemploController extends Controller
{
    public function texto()
    {
        return 'Hola mundo!';
    }

    public function vista(): View
    {
        return view('hola', [
            'texto' => 'Bienvenido al sistema'
        ]);
    }

    public function json(): JsonResponse
    {
        return new JsonResponse([
            'status' => 'success',
            'message' => 'Data retrieved successfully',
            'data' => [
                'id' => 1,
                'name' => 'Example Item',
            ],
        ]);
    }

}
```
### Vista

```php
@extends('layouts.app')
 
@section('title', 'Lista de productos')
 
@section('content')
    
<div class="card my-3">
  <div class="card-header">

    <div class="d-flex justify-content-between align-items-center">
    <p class="mb-0">Productos</p>

    <a class="btn btn-sm btn-primary" href="{{ url('/producto/create');}}">Agregar</a>
</div>

  </div>
  <div class="card-body">
    
    
    @if (session('status'))
    <div class="alert alert-success">
        {{ session('status') }}
    </div>
    @endif
    <table class="table table-bordered table-sm">
        <thead>
            <tr>
                <th class="text-center">SKU</th>
                <th class="text-center">NOMBRE</th>
                <th class="text-center">MARCA</th>
                <th class="text-center">PRECIO</th>
                <th class="text-center">ACCIONES</th>
            </tr>
        </thead>
        <tbody>
        @foreach ($productos as $item)
            <tr>
                <td>{{ $item->sku }}</td>
                <td>{{ $item->nombre }}</td>
                <td>{{ $item->marca }}</td>
                <td class="text-end">{{ number_format($item->precio,2) }}</td>
                <td class="text-center">
                    <a class="btn btn-sm btn-secondary" href="{{ url('/producto/'.$item->id);}}">Ver</a>
                    <a class="btn btn-sm btn-warning" href="{{ url('/producto/'.$item->id.'/edit');}}">Editar</a>
                </td>
            </tr>
        @endforeach
        </tbody>
    </table>

    {{ $productos->links() }}

    </div>
</div>

@endsection
```


