<?php include 'header.php';	?>

<h3><span class="glyphicon glyphicon-briefcase"></span>  Data Barang Terjual</h3>
<button style="margin-bottom:20px" data-toggle="modal" data-target="#myModal" class="btn btn-success col-md-2"><span class="glyphicon glyphicon-pencil"></span>  Tambah Penjualan</button>
<form action="barang_laku.php" method="get">
	<div class="input-group col-md-9 col-md-offset-5">
		<tr>
			<td><label>Dari</label>
			<td>: </td><input name="tgl1" type="text"  id="tanggal_awal" autocomplete="off"></td>
			<td><label>Tanggal Akhir</label>
			<td>: </td><input name="tgl2" type="text" id="tanggal_akhir" autocomplete="off"></td>
			<td>: </td>
			<td><input class="easyui-linkbutton c1" type="submit" name="Submit"  value="Proses">
		</tr>
	</div>

</form>
<?php
	if(isset($_POST['Submit'])){
		$tgl1 = $_GET['tgl1'];
		$tgl2 = $_GET['tgl2'];
	}
?>
<br/>
<br/>
<div class="panel panel-primary">
        <div class="panel-heading">
          <h3 class="panel-title"><b>Data Barang Terjual</b></h3>
        </div>
        <div class="panel-body">
          <div class="table-responsive">
            <table class="table table-striped table-hover">
	<tr>
		<th>No</th>
		<th>Tanggal</th>
		<th>Nama Barang</th>
		<th>Harga Terjual /pc</th>
		<th>Total Harga</th>
		<th>Jumlah</th>			
		<th>Laba</th>			
		<th>Opsi</th>
	</tr>
	<?php 
	if(isset($_GET['tgl1']) && isset ($_GET['tgl2'])){
		$tgl1=mysql_real_escape_string($_POST['tgl1']);
		$tgl2=mysql_real_escape_string($_POST['tgl2']);
		$brg=mysql_query("select * from barang_laku where tanggal between $tgl1 and $tgl2");
	}else{
		$brg=mysql_query("select * from barang_laku order by tanggal desc");
	}
	$no=1;
	if ($brg === FALSE) {
    die(mysql_error());
}
	while($b=mysql_fetch_array($brg)){

		?>
		<tr>
			<td><?php echo $no++ ?></td>
			<td><?php echo $b['tanggal'] ?></td>
			<td><?php echo $b['nama'] ?></td>
			<td>Rp.<?php echo number_format($b['harga']) ?>,-</td>
			<td>Rp.<?php echo number_format($b['total_harga']) ?>,-</td>
			<td><?php echo $b['jumlah'] ?></td>			
			<td><?php echo "Rp.".number_format($b['laba']).",-"?></td>			
			<td>		
				<a href="edit_laku.php?id=<?php echo $b['id']; ?>" class="btn btn-warning">Edit</a>
				<a onclick="if(confirm('Apakah anda yakin ingin menghapus data ini ??')){ location.href='hapus_laku.php?id=<?php echo $b['id']; ?>&jumlah=<?php echo $b['jumlah'] ?>&nama=<?php echo $b['nama']; ?>' }" class="btn btn-danger">Hapus</a>
			</td>
		</tr>

		<?php 
	}
	?>
	<tr>
		<td colspan="7">Total Pemasukan</td>
		<?php 
		if(isset($_GET['tanggal'])){
			$tanggal=mysql_real_escape_string($_GET['tanggal']);
			$x=mysql_query("select sum(total_harga) as total from barang_laku where tanggal='$tanggal'");	
			$xx=mysql_fetch_array($x);			
			echo "<td><b> Rp.". number_format($xx['total']).",-</b></td>";
		}else{

		}

		?>
	</tr>
	<tr>
		<td colspan="7">Total Laba</td>
		<?php 
		if(isset($_GET['tanggal'])){
			$tanggal=mysql_real_escape_string($_GET['tanggal']);
			$x=mysql_query("select sum(laba) as total from barang_laku where tanggal='$tanggal'");	
			$xx=mysql_fetch_array($x);			
			echo "<td><b> Rp.". number_format($xx['total']).",-</b></td>";
		}else{

		}

		?>
	</tr>
</table>

<!-- modal input -->
<div id="myModal" class="modal fade">
	<div class="modal-dialog">
		<div class="modal-content">
			<div class="modal-header">
				<button type="button" class="close" data-dismiss="modal" aria-hidden="true">&times;</button>
				<h4 class="modal-title">Tambah Penjualan
				</div>
				<div class="modal-body">				
					<form action="barang_laku_act.php" method="post">
						<div class="form-group">
							<label>Tanggal</label>
							<input name="tgl" type="text" class="form-control" id="tgl" autocomplete="off">
						</div>	
						<div class="form-group">
							<label>Nama Barang</label>								
							<select class="form-control" name="nama">
								<?php 
								$brg=mysql_query("select * from barang");
								while($b=mysql_fetch_array($brg)){
									?>	
									<option value="<?php echo $b['nama']; ?>"><?php echo $b['nama'] ?></option>
									<?php 
								}
								?>
							</select>

						</div>									
						<div class="form-group">
							<label>Harga Jual / unit</label>
							<input name="harga" type="text" class="form-control" placeholder="Harga" autocomplete="off">
						</div>	
						<div class="form-group">
							<label>Jumlah</label>
							<input name="jumlah" type="text" class="form-control" placeholder="Jumlah" autocomplete="off">
						</div>																	

					</div>
					<div class="modal-footer">
						<button type="button" class="btn btn-default" data-dismiss="modal">Batal</button>
						<input type="reset" class="btn btn-danger" value="Reset">												
						<input type="submit" class="btn btn-primary" value="Simpan">
					</div>
				</form>
			</div>
		</div>
	</div>
	<script type="text/javascript">
		$(document).ready(function(){
			$("#tanggal_awal").datepicker({dateFormat : 'yy/mm/dd'});							
		});
	</script>

	<script type="text/javascript">
		$(document).ready(function(){
			$("#tanggal_akhir").datepicker({dateFormat : 'yy/mm/dd'});							
		});
	</script>
	<?php include 'footer.php'; ?>
