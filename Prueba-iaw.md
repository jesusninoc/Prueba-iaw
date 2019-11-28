```PowerShell
$titulos = echo "Bolsa"

Foreach ($reloj in $titulos){

   
    New-Item -ItemType Directory -Name $reloj -Force
    cd C:\xampp\htdocs\wp\$reloj
    php.exe C:\xampp\php\wp-cli.phar core download
    php.exe C:\xampp\php\wp-cli.phar config create --dbname=basedatos$reloj --dbuser=root
    php.exe C:\xampp\php\wp-cli.phar db create
    php.exe C:\xampp\php\wp-cli.phar core install --url=127.0.0.1/wp/$reloj --title="$reloj" --admin_user=root --admin_password=1928 --admin_email=admin@email.com 
    cd ..
}
#########
$web = Invoke-WebRequest  "http://www.bolsamadrid.es/esp/aspx/Mercados/Precios.aspx?indice=ESI100000000"
$bancos= $web.AllElements | where class -EQ "DifFlsb" | select  -Skip 1
$bancos.innerText > bancos.txt
$contenido = gc .\bancos.txt
cd C:\xampp\htdocs\wp\$reloj
php.exe C:\xampp\php\wp-cli.phar post create --post_title="Bancos" --post_content="$contenido" --post_type="post" --menu_order=7 

##########################
for(1){
$web = Invoke-WebRequest "http://www.bolsamadrid.es/esp/aspx/Mercados/Precios.aspx?indice=ESI100000000"
$web.Content  > sample.txt
$prueba = gc .\sample.txt

$prueba |Select-String -Pattern "/esp/aspx/Empresas/FichaValor.aspx?"   >> Nombres.txt

echo "Bolsa Madrid" > titulo.txt

$hora = (Get-Date  -Format d)
###################################crea los post 
foreach ($contenido in $prueba |Select-String -Pattern "/esp/aspx/Empresas/FichaValor.aspx?")
{%{
cd C:\xampp\htdocs\wp\$reloj
php.exe C:\xampp\php\wp-cli.phar post create --post_title="$hora" --post_content="<table>
<tr><td>Nombre</td><td>Ult</td><td>% dif.</td><td>Max</td><td>Min</td><td>Volumen</td><td>Efectivo</td><td>Fecha</td><td>Hora</td></tr>$contenido</table>" --post_type="post" --menu_order=7 }
}
rm .\Nombres.txt
rm .\sample.txt
 Start-Sleep -Seconds 86.400
}

```
