# ATKATKw
https://drive.google.com/drive/folders/1z39DWkTDZCT91NT5DKJWxRdrrBdHyWsd?usp=sharing

#########view creat bank @extends('layouts.master')

@section('stylesheets')
<link href="{{ url('css/jquery.gritter.css') }}" rel="stylesheet">
<link href="{{ url('css/select2.min.css') }}" rel="stylesheet">
<style>


.form-group {
    display: flex;
    align-items: center;
    margin-bottom: 15px;
}
.form-group label {
    width: 150px;
    margin-right: 10px;
    font-weight: bold;
}
.form-group .form-control {
    flex: 1;
}
.error-message {
    color: red;
    font-size: 13px;
    margin-top: 5px;
}
    /* Biar radio inline sejajar dan jarak antar pilihan rapih */
.radio-inline {
    display: inline-flex;
    align-items: center;
    margin-right: 15px; /* jarak antar pilihan */
    font-weight: 600;
    font-size: 14px; /* samakan dengan label utama */
    cursor: pointer;
}

.radio-inline input[type="radio"] {
    margin-right: 5px; /* jarak radio dengan teks */
}

/* Override agar label utama dan input sejajar */
.form-group.row > label.control-label {
    font-weight: 600;
    font-size: 14px;
}
    .form-group label {
        padding-left: 7%;
        display: block;
        font-weight: 600;
        padding-top: 7px;
    }
    .btn-group-center {
        text-align: center;
        margin-top: 20px;
    }
    .error-message {
        color: red;
        font-size: 12px;
        margin-top: 5px;
    }
    .alert-error {
        color: red;
        font-weight: bold;
        margin-bottom: 10px;
    }

    /* Styling tombol kecil dan proporsional */
    .btn-small {
        font-size: 14px;
        padding: 6px 20px;
        font-weight: normal;
    }

    /* Container tombol submit dan cancel */
    .btn-action-group {
        display: flex;
        justify-content: center;
        gap: 10px; /* jarak antar tombol */
        margin-top: 20px;
    }
</style>
@endsection

@section('content')
<div class="container">
    <h2 class="text-primary" style="margin-bottom: 20px;">Add Bank</h2>

    <div style="
        background-color: white;
        padding: 20px 30px;
        border-radius: 5px;
        box-shadow: 0 0 8px rgba(0,0,0,0.1);
        border-top: 5px solid #337ab7;
    ">
        <h4 class="text-center" style="color: #000000ff; font-size: 20px; margin-bottom: 25px;">Bank Data</h4>

        @if(session('success'))
            <div class="alert alert-success">
                {{ session('success') }}
            </div>
        @endif

        {{-- Pesan error umum --}}
        @if ($errors->any())
            <div class="alert-error">
                Data belum lengkap, silakan isi semua field yang wajib.
            </div>
        @endif

        <form action="{{ route('bank.store') }}" method="POST">
            {{ csrf_field() }}

            <div class="row">
                <!-- Kolom Kiri -->
                <div class="col-md-6">

                <!-- <select name="vendor" id="vendor" class="form-control"> -->

                 <!-- Vendor -->
<div class="form-group row">
    <label for="vendor" class="col-sm-4 control-label">Vendor</label>
    <div class="col-sm-8">
        <select class="form-control" name="vendor" id="vendor">
            <option value="">-- Select Vendor --</option>
            @foreach ($vendors as $vendor)
                <option value="{{ $vendor->id }}" {{ old('vendor', $rekening->vendor) == $vendor->id ? 'selected' : '' }}>
                    {{ $vendor->supplier_name }}
                </option>
            @endforeach
        </select>
        @if ($errors->has('vendor'))
            <div class="error-message">{{ $errors->first('vendor') }}</div>
        @endif
    </div>
</div>

  <!-- Currency -->
<div class="form-group row">
    <label for="currency" class="col-sm-4 control-label">Currency</label>
    <div class="col-sm-8">
        <select class="form-control" name="currency" id="currency">
            <option value="">-- Pilih Currency --</option>
            <option value="IDR" {{ old('currency', $rekening->currency) == 'IDR' ? 'selected' : '' }}>IDR</option>
            <option value="USD" {{ old('currency', $rekening->currency) == 'USD' ? 'selected' : '' }}>USD</option>
            <option value="JPY" {{ old('currency', $rekening->currency) == 'JPY' ? 'selected' : '' }}>JPY</option>
        </select>
        @if ($errors->has('currency'))
            <div class="error-message">{{ $errors->first('currency') }}</div>
        @endif
    </div>
</div>





                    @php
                        $leftFields = [
                            'branch' => 'Branch',
                            'rekening_no' => 'No Rekening',
                            'rekening_nama' => 'Nama Rekening',
                            'address_vendor' => 'Address Vendor',
                            'city' => 'City',
                            'country' => 'Country',
                            'bank_address' => 'Bank Address',
                            'bank_name' => 'Bank Name',
                            'bank_branch' => 'Bank Branch',
                            'bank_city_country' => 'Bank City Country',
                        ];
                    @endphp
@foreach($leftFields as $name => $label)
    <div class="form-group row">
        <label for="{{ $name }}" class="col-sm-4 control-label">{{ $label }}</label>
        <div class="col-sm-8">
            <input type="text" class="form-control" name="{{ $name }}" id="{{ $name }}" value="{{ old($name, isset($rekening) ? $rekening->$name : '') }}">
            
            @if ($errors->has($name))
                <div class="error-message">{{ $errors->first($name) }}</div>
            @endif
        </div>
    </div>
@endforeach

                  
                </div>

                
 <!-- Internal -->
<div class="form-group row">
    <label for="internal" class="col-sm-4 control-label">Internal</label>
    <div class="col-sm-8">
        <select class="form-control" name="internal" id="internal">
            <option value="">-- Pilih Internal --</option>
            <option value="true" {{ old('internal', $rekening->internal) == 'True' ? 'selected' : '' }}>true</option>
            <option value="false" {{ old('internal', $rekening->internal) == 'False' ? 'selected' : '' }}>false</option>
        </select>
        @if ($errors->has('internal'))
            <div class="error-message">{{ $errors->first('internal') }}</div>
        @endif
    </div>
</div>

<!-- LN -->
<div class="form-group row">
    <label for="ln" class="col-sm-4 control-label">LN</label>
    <div class="col-sm-8">
        <select class="form-control" name="ln" id="ln">
            <option value="">-- Pilih LN --</option>
            <option value="true" {{ old('ln', $rekening->ln) == 'True' ? 'selected' : '' }}>true</option>
            <option value="false" {{ old('ln', $rekening->ln) == 'False' ? 'selected' : '' }}>false</option>
        </select>
        @if ($errors->has('ln'))
            <div class="error-message">{{ $errors->first('ln') }}</div>
        @endif
    </div>
</div>

<!-- Not Active -->
<div class="form-group row">
    <label for="not_active" class="col-sm-4 control-label">Not Active</label>
    <div class="col-sm-8">
        <select class="form-control" name="not_active" id="not_active">
            <option value="">-- Pilih Not Active --</option>
            <option value="true" {{ old('not_active', $rekening->not_active) == 'True' ? 'selected' : '' }}>true</option>
            <option value="false" {{ old('not_active', $rekening->not_active) == 'False' ? 'selected' : '' }}>false</option>
        </select>
        @if ($errors->has('not_active'))
            <div class="error-message">{{ $errors->first('not_active') }}</div>
        @endif
    </div>
</div>

                <!-- Kolom Kanan -->
                <div class="col-md-6">
                    @php
                        $rightFields = [
                            'switch_code' => 'Switch Code',
                            'full_amount' => 'Full Amount',
                            'settle_acc_no' => 'Settle Acc No',
                        ];
                    @endphp

                    @foreach($rightFields as $name => $label)
                        <div class="form-group row">
                            <label for="{{ $name }}" class="col-sm-4 control-label">{{ $label }}</label>
                            <div class="col-sm-8">
                                <input type="text" class="form-control" name="{{ $name }}" id="{{ $name }}" value="{{ old($name) }}">
                                @if ($errors->has($name))
                                    <div class="error-message">{{ $errors->first($name) }}</div>
                                @endif
                            </div>
                        </div>
                    @endforeach




                    <!-- Resident -->
                    <div class="form-group row">
                        <label for="resident" class="col-sm-4 control-label">Resident</label>
                        <div class="col-sm-8">
                            <input type="text" class="form-control" name="resident" id="resident" value="{{ old('resident') }}">
                            @if ($errors->has('resident'))
                                <div class="error-message">{{ $errors->first('resident') }}</div>
                            @endif
                        </div>
                    </div>

                    <!-- Citizenship -->
                    <div class="form-group row">
                        <label for="citizenship" class="col-sm-4 control-label">Citizenship</label>
                        <div class="col-sm-8">
                            <input type="text" class="form-control" name="citizenship" id="citizenship" value="{{ old('citizenship') }}">
                            @if ($errors->has('citizenship'))
                                <div class="error-message">{{ $errors->first('citizenship') }}</div>
                            @endif
                        </div>
                    </div>

                    <!-- Relation -->
                    <div class="form-group row">
                        <label for="relation" class="col-sm-4 control-label">Relation</label>
                        <div class="col-sm-8">
                            <input type="text" class="form-control" name="relation" id="relation" value="{{ old('relation') }}">
                            @if ($errors->has('relation'))
                                <div class="error-message">{{ $errors->first('relation') }}</div>
                            @endif
                        </div>
                    </div>

                    <!-- Sector -->
<div class="form-group row">
    <label for="sector_select" class="col-sm-4 control-label">Sector</label>
    <div class="col-sm-8">
        <select class="form-control" name="sector_select" id="sector_select">
            <option value="">-- Pilih Sector --</option>
            <option value="domestic" {{ old('sector_select', $rekening->sector_select) == 'Domestic' ? 'selected' : '' }}>domestic</option>
            <option value="book transfer" {{ old('sector_select', $rekening->sector_select) == 'Book Transfer' ? 'selected' : '' }}>book transfer</option>
            <option value="internasional" {{ old('sector_select', $rekening->sector_select) == 'Internasional' ? 'selected' : '' }}>internasional</option>
        </select>
        @if ($errors->has('sector_select'))
            <div class="error-message">{{ $errors->first('sector_select') }}</div>
        @endif
    </div>
</div>


                    <!-- Bank Charge -->
                    <div class="form-group row">
                        <label for="bank_charge" class="col-sm-4 control-label">Bank Charge</label>
                        <div class="col-sm-8">
                            <select class="form-control" name="bank_charge" id="bank_charge">
                                <option value="">-- Pilih Bank Charge --</option>
                                <option value="OUR" {{ old('bank_charge') == 'OUR' ? 'selected' : '' }}>OUR</option>
                                <option value="SHA" {{ old('bank_charge') == 'SHA' ? 'selected' : '' }}>SHA</option>
                            </select>
                            @if ($errors->has('bank_charge'))
                                <div class="error-message">{{ $errors->first('bank_charge') }}</div>
                            @endif
                        </div>
                    </div>
                </div>
            </div>

            <!-- Tombol Submit dan Cancel -->
            <div class="row form-group btn-group-center">
                <div class="col-sm-12" style="display: flex; justify-content: center; gap: 10px;">
                    <button type="submit" class="btn btn-success" style="width: 100px;">Submit</button>
                    <a href="{{ route('bank.index') }}" class="btn btn-default" style="width: 100px;">Cancel</a>
                </div>
            </div>

        </form>
    </div>
</div>
@endsection

@section('scripts')
<script src="{{ url('js/select2.min.js') }}"></script>
<script>
    $(document).ready(function() {
        $('.select2').select2();
    });
</script>
@endsection


############### edit bank
@extends('layouts.master')

@section('stylesheets')
<link href="{{ url('css/jquery.gritter.css') }}" rel="stylesheet">
<link href="{{ url('css/select2.min.css') }}" rel="stylesheet">
<style>
    .form-group label {
        padding-left: 7%;
        display: block;
        font-weight: 600;
        padding-top: 7px;
        /* max-width: 150px; */
    }
    .btn-group-center {
        text-align: center;
        margin-top: 20px;
    }
    .error-message {
        color: red;
        font-size: 12px;
        margin-top: 5px;
        /* padding-left: 7%; */
    }
    .alert-error {
        color: red;
        font-weight: bold;
        margin-bottom: 10px;
    }

    /* Styling tombol kecil dan proporsional */
    .btn-small {
        font-size: 14px;
        padding: 6px 20px;
        font-weight: normal;
    }

    /* Container tombol submit dan cancel */
    .btn-action-group {
        display: flex;
        justify-content: center;
        gap: 10px;
        margin-top: 20px;
    }

    /* Container form mirip canvas */
    .form-canvas {
        background-color: white;
        padding: 20px 30px;
        border-radius: 5px;
        box-shadow: 0 0 8px rgba(0,0,0,0.1);
        border-top: 5px solid #337ab7;
        /* max-width: 700px; */
        margin: 0 auto 30px auto;
    }

    /* Atur lebar input agar tidak terlalu panjang */
    .form-control {
 max-width: 250px; /* Atur panjang maksimal input */
    width: 100%;     }

    /* Label dan input berdampingan */
    .form-group {
        display: flex;
        align-items: center;
        margin-bottom: 15px;
    }
    .form-group label {
        flex: 0 0 160px;
        padding-left: 0;
        text-align: left;
    }
    .form-group .form-control,
    .form-group select {
        flex: 1 1 auto;
        max-width: 250px;
    }
</style>
@endsection

@section('content')
<div class="container">
    <h2 class="text-primary" style="margin-bottom: 20px;">Edit Bank</h2>

    <div class="form-canvas">
        <h4 class="text-center" style="color: #000000ff; font-size: 20px; margin-bottom: 25px;">Bank Data</h4>

        @if(session('success'))
            <div class="alert alert-success">
                {{ session('success') }}
            </div>
        @endif

        {{-- Pesan error umum --}}
        @if ($errors->any())
            <div class="alert-error">
                Data belum lengkap, silakan isi semua field yang wajib.
            </div>
        @endif

        <form action="{{ route('update.bank', $rekening->id) }}" method="POST" autocomplete="off">
            {{ csrf_field() }}
            {{ method_field('PUT') }}

            <div class="row">
                <!-- Kolom Kiri -->
                <div class="col-md-6">

                
                    @php
                        $leftFields = [
                            'vendor' => 'Vendor Name',
                            'currency' => 'Currency',
                            'branch' => 'Branch',
                            'rekening_no' => 'No Rekening',
                            'rekening_nama' => 'Nama Rekening',
                            'internal' => 'Internal',
                            'ln' => 'LN',
                            'not_active' => 'Not Active',
                            'address_vendor' => 'Vendor Address',
                            'city' => 'City',
                            'country' => 'Country',
                            'bank_address' => 'Bank Address'
                        ];
                    @endphp

                    @foreach($leftFields as $name => $label)
                        <div class="form-group">
                            <label for="{{ $name }}">{{ $label }}</label>
                            <input type="text" class="form-control" name="{{ $name }}" id="{{ $name }}" value="{{ old($name, $rekening->$name) }}">
                            @if ($errors->has($name))
                                <div class="error-message">{{ $errors->first($name) }}</div>
                            @endif
                        </div>
                    @endforeach
                </div>

                <!-- Kolom Kanan -->
                <div class="col-md-6">
                    @php
                        $rightFields = [
                            'bank_name' => 'Bank Name',
                            'bank_branch' => 'Bank Branch',
                            'bank_city_country' => 'Bank City Country',
                            'switch_code' => 'Switch Code',
                            'full_amount' => 'Full Amount',
                            'settle_acc_no' => 'Settle Acc No',
                            'sector_select' => 'Sector Select',
                            'resident' => 'Resident',
                            'citizenship' => 'Citizenship',
                            'relation' => 'Relation',
                            'bank_charge' => 'Bank Charge'
                        ];
                    @endphp

                    @foreach($rightFields as $name => $label)
                        @if(in_array($name, ['sector_select', 'bank_charge']))
                            <div class="form-group">
                                <label for="{{ $name }}">{{ $label }}</label>
                                <select class="form-control" name="{{ $name }}" id="{{ $name }}">
                                    <option value="">-- Pilih {{ $label }} --</option>
                                    @if($name == 'sector_select')
                                        <option value="domestic" {{ old($name, $rekening->$name) == 'domestic' ? 'selected' : '' }}>Domestic</option>
                                        <option value="book transfer" {{ old($name, $rekening->$name) == 'book transfer' ? 'selected' : '' }}>Book Transfer</option>
                                        <option value="internasional" {{ old($name, $rekening->$name) == 'internasional' ? 'selected' : '' }}>Internasional</option>
                                    @elseif($name == 'bank_charge')
                                        <option value="OUR" {{ old($name, $rekening->$name) == 'OUR' ? 'selected' : '' }}>OUR</option>
                                        <option value="SHA" {{ old($name, $rekening->$name) == 'SHA' ? 'selected' : '' }}>SHA</option>
                                    @endif
                                </select>
                                @if ($errors->has($name))
                                    <div class="error-message">{{ $errors->first($name) }}</div>
                                @endif
                            </div>
                        @else
                            <div class="form-group">
                                <label for="{{ $name }}">{{ $label }}</label>
                                <input type="text" class="form-control" name="{{ $name }}" id="{{ $name }}" value="{{ old($name, $rekening->$name) }}">
                                @if ($errors->has($name))
                                    <div class="error-message">{{ $errors->first($name) }}</div>
                                @endif
                            </div>
                        @endif
                    @endforeach
                </div>
            </div>

            <div class="btn-action-group">
                <button type="submit" class="btn btn-success btn-small" style="width: 100px;">Update</button>
                <a href="{{ url()->previous() }}" class="btn btn-default btn-small" style="width: 100px;">Cancel</a>
            </div>
        </form>
    </div>
</div>
@endsection

@section('scripts')
<script src="{{ url('js/select2.min.js') }}"></script>
<script>
    $(document).ready(function() {
        $('.select2').select2();
    });
</script>
@endsection


########view data bank 
@extends('layouts.master')

@section('stylesheets')
<link href="{{ url('css/jquery.gritter.css') }}" rel="stylesheet">
<link href="{{ url('css/jquery.tagsinput.css') }}" rel="stylesheet">
<style type="text/css">
    thead input {
        width: 100%;
        padding: 3px;
        box-sizing: border-box;
    }                               
    #listTableBody > tr:hover {
        cursor: pointer;
        background-color: #7dfa8c;
    }
    table.table-bordered {
        border:1px solid black;
        vertical-align: middle; 
    }
    table.table-bordered > thead > tr > th {
        border:1px solid black;
        vertical-align: middle;
    }
    table.table-bordered > tbody > tr > td {
        border:1px solid rgb(150,150,150);
        vertical-align: middle;
    }
    .dataTables_wrapper .dt-buttons {
        float: left;
        margin-bottom: 10px;
    }
    .row-info {
        margin-top: 15px;
        font-weight: bold;
    }
</style>
@stop

@section('header')
<section class="content-header">
    <h1>
        Bank List YMPI
    </h1>
    <ol class="breadcrumb"></ol>
</section>
@stop

@section('content')
<section class="content">
    <div class="box">
        <div class="box-body" style="overflow-x:scroll;">
            <a href="{{ route('bank.create') }}" class="btn btn-primary add-bank-btn">
                <i class="fa fa-plus"></i> Add Bank
            </a>

            <div class="row-info">
                Showing {{ count($accRekenings) }} entries
            </div>

            <table id="listTable" class="table table-bordered table-striped table-hover">
                <thead style="background-color: rgba(126,86,134,.7);">
                    <tr>
                        <th>Vendor Name</th>
                        <th>Currency</th>
                        <th>Bank & Branch Name</th>
                        <th>Nama Rekening</th>
                        <th>No Rekening</th>
                        <th>Internal</th>
                        <th>LN</th>
                        <th>Not Active</th>
                        <th>Vendor Address</th>
                        <th>City</th>
                        <th>Country</th>
                        <th>Bank Address</th>
                        <th>Bank Name</th>
                        <th>Bank Branch</th>
                        <th>Bank City Country</th>
                        <th>Switch Code</th>
                        <th>Full Amount</th>
                        <th>Sett Acc No</th>
                        <th>Sector Select</th>
                        <th>Resident</th>
                        <th>Citizenship</th>
                        <th>Relation</th>
                        <th>Bank Charge</th>
                        <th>Action</th>
                    </tr>
                </thead>
                <tbody id="listTableBody">
                    @foreach($accRekenings as $rekening)
                    <tr>
                        <td>{{ $rekening->vendor }}</td>
                        <td>{{ $rekening->currency }}</td>
                        <td>{{ $rekening->branch }}</td>
                        <td>{{ $rekening->rekening_nama }}</td>
                        <td>{{ $rekening->rekening_no }}</td>
                        <td>{{ $rekening->internal }}</td>
                        <td>{{ $rekening->ln }}</td>
                        <td>{{ $rekening->not_active }}</td>
                        <td>{{ $rekening->address_vendor }}</td>
                        <td>{{ $rekening->city }}</td>
                        <td>{{ $rekening->country }}</td>
                        <td>{{ $rekening->bank_address }}</td>
                        <td>{{ $rekening->bank_name }}</td>
                        <td>{{ $rekening->bank_branch }}</td>
                        <td>{{ $rekening->bank_city_country }}</td>
                        <td>{{ $rekening->switch_code }}</td>
                        <td>{{ $rekening->full_amount }}</td>
                        <td>{{ $rekening->settle_acc_no }}</td>
                        <td>{{ $rekening->sector_select }}</td>
                        <td>{{ $rekening->resident }}</td>
                        <td>{{ $rekening->citizenship }}</td>
                        <td>{{ $rekening->relation }}</td>
                        <td>{{ $rekening->bank_charge }}</td>
                        <td>
                           
                            <a href="{{ route('edit.bank', $rekening->id) }}" class="btn btn-xs btn-warning">
                                <i class="fa fa-edit"></i> Edit
                            </a>

                            <button class="btn btn-xs btn-danger" onclick="deleteBank({{ $rekening->id }})">
                                <i class="fa fa-trash"></i> Delete
                            </button>
                        </td>
                    </tr>
                    @endforeach
                </tbody>
            </table>
        </div>
    </div>  
</section>
@stop

@section('scripts')
<script src="{{ url('js/jquery.gritter.min.js') }}"></script>
<script src="{{ url('js/buttons.flash.min.js') }}"></script>
<script src="{{ url('js/jszip.min.js') }}"></script>
<script src="{{ url('js/vfs_fonts.js') }}"></script>
<script src="{{ url('js/buttons.html5.min.js') }}"></script>
<script src="{{ url('js/buttons.print.min.js') }}"></script>
<script src="{{ url('js/jquery.tagsinput.min.js') }}"></script>

<script>
    $(document).ready(function() {
        var table = $('#listTable').DataTable({
            paging: true,
            searching: true,
            ordering: true,
            info: true,
            autoWidth: false,
            dom: 'Bfrtip',
            buttons: [
                {
                    extend: 'copy',
                    text: '<i class="fa fa-copy"></i> Copy',
                    className: 'btn btn-success',
                    exportOptions: {
                        columns: ':not(:last-child)'
                    }
                },
                {
                    extend: 'excel',
                    text: '<i class="fa fa-file-excel-o"></i> Excel',
                    className: 'btn btn-info',
                    exportOptions: {
                        columns: ':not(:last-child)'
                    }
                },
                {
                    extend: 'pdf',
                    text: '<i class="fa fa-file-pdf-o"></i> PDF',
                    className: 'btn btn-danger',
                    exportOptions: {
                        columns: ':not(:last-child)'
                    }
                },
                {
                    extend: 'print',
                    text: '<i class="fa fa-print"></i> Print',
                    className: 'btn btn-warning',
                    exportOptions: {
                        columns: ':not(:last-child)'
                    }
                }
            ],
            initComplete: function() {
                console.log('DataTable buttons initialized');
            }
        });

        $('#listTable').on('draw.dt', function () {
            var info = table.page.info();
            $('.row-info').text('Showing ' + (info.start + 1) + ' to ' + info.end + ' of ' + info.recordsTotal + ' entries');
        });
    });

    function deleteBank(id) {
        if(confirm('Are you sure you want to delete this bank record?')) {
            $.ajax({
                url: '{{ url("bank") }}/' + id,
                type: 'DELETE',
                data: {
                    _token: '{{ csrf_token() }}'
                },
                success: function(response) {
                    alert('Bank deleted successfully.');
                    location.reload();
                },
                error: function(xhr) {
                    alert('Failed to delete bank.');
                }
            });
        }
    }
</script>
@stop

