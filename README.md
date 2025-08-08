# ATKATKw
https://drive.google.com/drive/folders/1z39DWkTDZCT91NT5DKJWxRdrrBdHyWsd?usp=sharing

view

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Store Data</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css">
    <style>
        .form-container {
            margin-top: 50px;
        }
        .form-group label {
            font-weight: bold;
        }
    </style>
</head>
<body>

<div class="container form-container">
    <h2>Data yang Dikirimkan</h2>

    <div class="alert alert-success">
        Data telah berhasil dikirimkan! Berikut adalah data yang Anda kirimkan:
    </div>

    <table class="table table-bordered">
        <thead class="thead-dark">
            <tr>
                <th>Field</th>
                <th>Value</th>
            </tr>
        </thead>
        <tbody>
            @foreach ($data as $key => $value)
                <tr>
                    <td>{{ ucfirst(str_replace('_', ' ', $key)) }}</td>
                    <td>{{ $value }}</td>
                </tr>
            @endforeach
        </tbody>
    </table>

    <a href="{{ route('suppliers.create') }}" class="btn btn-primary">Kembali ke Form</a>
</div>

</body>
</html>

