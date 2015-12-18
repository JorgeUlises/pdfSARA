# pdfSARA
Esta extensión permite realizar PDF a partir de plantillas HTML.

Se llaman como un componente del Formulario de Sara extendiendo en Frontera.class.php

```php
/*
 * Sirve para agregar al core de SARA la funcionalidad de plantillas con domPDF
 */
include_once("core/builder/FormularioHtml.class.php");

if (class_exists ( '\FormularioHtml' )) {
	class FormularioHtml extends \FormularioHtml {
		function __construct() {
			/**
			 * Se agregan los componentes hechos con Boostrap para SARA
			 */
			require_once ($this->ruta . "builder/DomPdf.class.php");
			// use blocks\docentes\planDeTrabajo\builder\componentes;
			//Se llama a la clase constructor del padre
			parent::__construct ();
			//Se llama a las funciones que están dentro de la clase y se agregan al formulario
			$this->aggregate ( 'DomPdfPlugin' );
			//Se termina la agregación
		}
	}
	
	class Frontera {
  	....
  	function html(){
  	...
  	        $this->miFormulario = new FormularioHtml();
  	...
  	}
	}
```

Luego en el formulario se llama:

```php
$cadenaSql = $this->miSql->getCadenaSql ( 'novedades_bonificacion', $documento );
$resultado = $esteRecursoDB->ejecutarAcceso ( $cadenaSql, 'busqueda' );
$items[] = array(
		'resultados' => ($resultado)?$resultado:array(),
		'tipo' => '2',
		'tituloTipo' => 'Bonificación',
		'titulo' => 'Novedades',
		'titulo1' => 'Novedad',
		'llavevalor1' => 'tipo_trabajogrado',
		'llavevalor2' => 'titulo_trabajogrado'
);

$atributos ['id'] = 'reportePDFDocente'; // No cambiar este nombre
$atributos ['plantilla'] = 'plantilla1.html.php';
$atributos ['datos_docente'] = $datosDocente;
$atributos ['items'] = $items;
$atributos ['showHTML'] = true;

$atributos ['destino'] = $documento;
echo '<div style="max-width:1024px;margin: 0 auto;">';
echo $this->miFormulario->pdf ( $atributos );
echo '</div>';
unset ( $atributos );
```
