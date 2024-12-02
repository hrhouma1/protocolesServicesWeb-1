- faire

```html
cd /var/www/html
rm -rf index.php
nano index.php
```

- et remplacez le contenu par le suivant :

```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title>Ajax CRUD Operation using PDO with Bootstrap/Modal</title>
	<!-- Bootstrap CDN -->
	<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">
</head>
<body>
<div class="container">
	<h1 class="page-header text-center">Ajax CRUD Operation using PDO</h1>
	<div class="row">
		<div class="col-sm-8 col-sm-offset-2">
			<button id="addnew" class="btn btn-primary"><span class="glyphicon glyphicon-plus"></span> New</button>
            <div id="alert" class="alert alert-info text-center" style="margin-top:20px; display:none;">
            	<button class="close"><span aria-hidden="true">&times;</span></button>
                <span id="alert_message"></span>
            </div>  
			<table class="table table-bordered table-striped" style="margin-top:20px;">
				<thead>
					<th>ID</th>
					<th>Firstname</th>
					<th>Lastname</th>
					<th>Address</th>
					<th>Action</th>
				</thead>
				<tbody id="tbody"></tbody>
			</table>
		</div>
	</div>
</div>
<!-- Modals -->
<?php include('modal.html'); ?>
<!-- jQuery and Bootstrap CDN -->
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/js/bootstrap.min.js"></script>
<script src="app.js"></script>
</body>
</html>
```

### Modifications apportées :
1. Le lien local vers `bootstrap.min.css` a été remplacé par le lien CDN :  
   ```html
   <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">
   ```

2. Les fichiers JavaScript locaux pour jQuery et Bootstrap ont été remplacés par leurs liens CDN :  
   ```html
   <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
   <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/js/bootstrap.min.js"></script>
   ```

Cela garantit que les styles et les scripts Bootstrap sont chargés correctement, même si les fichiers locaux ne sont pas accessibles.
