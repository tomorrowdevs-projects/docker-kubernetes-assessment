### Migrazione di un'Applicazione Laravel + MariaDB su Docker e Kubernetes

#### Obiettivo:
Preparare e migrare un'applicazione Laravel esistente, con un backend MariaDB, in un ambiente Docker e Kubernetes. Configurare l'architettura per garantire High Availability (HA) con un cluster MariaDB che supporti la replicazione per la lettura.

#### Codice di Partenza:
Il codice di partenza fornisce una semplice applicazione Laravel che gestisce un elenco di utenti. Di seguito, sono forniti i dettagli essenziali per iniziare:

- **Modello User (`app/Models/User.php`):**
```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    use HasFactory;

    protected $fillable = ['name', 'email', 'password'];
}
```

- **UserController (`app/Http/Controllers/UserController.php`):**
```php
<?php

namespace App\Http\Controllers;

use App\Models\User;

class UserController extends Controller
{
    public function index()
    {
        $users = User::all();
        return view('users.index', compact('users'));
    }
}
```

- **Vista (`resources/views/users/index.blade.php`):**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Users</title>
</head>
<body>
    <h1>Users</h1>
    <ul>
        @foreach ($users as $user)
            <li>{{ $user->name }} - {{ $user->email }}</li>
        @endforeach
    </ul>
</body>
</html>
```

- **Database Configuration (`.env`):**
```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=
```

#### Tasks:

1. **Containerizzare l'Applicazione Laravel:**
   Crea un `Dockerfile` per l'applicazione Laravel che ne permetta l'esecuzione in un container Docker. Utilizza le immagini ufficiali di PHP e Composer per configurare l'ambiente.

3. **Preparare il Database MariaDB su Docker:**
   Utilizza l'immagine ufficiale di MariaDB per eseguire il tuo database in un container Docker. Assicurati di configurare i volumi per la persistenza dei dati.

4. **Migrazione su Kubernetes:**
   - Prepara i file di configurazione YAML per Kubernetes, inclusi Deployment, Service, e eventualmente ConfigMap o Secrets per gestire le configurazioni e i dati sensibili.
   - Studia come implementare un cluster MariaDB per l'HA, potresti considerare l'uso di operatori Kubernetes specifici per MariaDB o soluzioni già pronte come Bitnami MariaDB Helm chart.
