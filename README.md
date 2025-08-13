# ATKATKw
https://drive.google.com/drive/folders/1z39DWkTDZCT91NT5DKJWxRdrrBdHyWsd?usp=sharing

vendor dropdown
currency IDR USD JPY
pilihan true/false INTERNAL, LN, NOT ACTIVE, 
sector : domestic , book transfer , internasional 
bank charge our sha 

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
