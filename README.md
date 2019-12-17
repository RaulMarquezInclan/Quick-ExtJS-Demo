# Quick-ExtJS-Demo
Demo de ExtJS que muestra las funcionalidades de los componentes grid, forma y boton. En el codigo se encuentran comentarios que ayudan a clarificar ciertas partes de la funcionalidad.

El proposito de este demo es tener una referencia basica del funcionamiento de las caracteristicas mas utilizadas del framework. Si a alguien le gustaria cubrir algun tema en particular se pueden comunicar conmigo al correo raulmarquezi@gmail.com.

## Pasos para ejecutar el demo

1. Utilizando el navegador entrar a la siguiente liga (nos hara disponible al framework):
https://docs.sencha.com/extjs/4.2.3/extjs-build/examples/build/KitchenSink/ext-theme-classic/#login-form

2. Abrir la consola del navegador (comunmente con la tecla F12 o con el menu contextual dentro del area de la pagina)

3. Pegar el siguiente codigo en la consola y presionar enter.

```
// Store para llenar el combo de marcas de autos
var storeMarcas = Ext.create('Ext.data.Store', {
    // Para propositos del llenado del combo en cuestion,
    // no sera necesario establecer los tipos de datos de
    // los 'fields', sinembargo, mas adelnate si lo haremos
    fields: ['id', 'Marca'],
    data: [
        { 'id': 1, 'Marca': 'Audi' },
        { 'id': 2, 'Marca': 'Tesla' },
        { 'id': 3, 'Marca': 'GMC' },
        { 'id': 4, 'Marca': 'Toyota' }
    ]
});

// Modelo que representara a un registro de auto,
// se utilizara para el store de autos
//
// NOTAS:
// - La configuracion 'mapping' de cada field del modelo solo es
//   necesaria cuando el nombre de la propiedad que recibimos del
//   backend es diferente a lo establecido en la configuracion
//   'name', es decir, si omitimos la configuracion 'mapping',
//   esta tomara por default el valor de la configuracion 'name'.
//
// - Es deseable el establecer el tipo de dato de cada campo del 
//   modelo, esto para facilitar el formato que le daremos a la
//   informacion y como mostrarla.

Ext.define('modeloAutos', {
    extend: 'Ext.data.Model',
    fields: [
        { name: 'id', type: 'int' },
        { name: 'Marca', type: 'string' },
        { name: 'Linea', type: 'string' },
        { name: 'Modelo', type: 'int' },
        { name: 'FechaIngreso', 
		  type: 'date',
          // Los controllers de MVC/.NET te regresan las
          // fechas en este formato: /Date(248362784632)/
          // 
          // Al configurar 'dateFormat' a 'MS', el modelo
          // podra interpretar ese dato correctamente como 
          // fecha
          //
          // Puede omitirse esta configuracion o estable-
          // cerse otro formato segun sea necesario
		  dateFormat: 'MS' },
        { name: 'Precio', type: 'float' }
    ]
});

// Store que sera utilizado por el grid de autos,
// se le asignan registros localmente con el
// proposito de visualizar datos en el grid
var storeAutos = Ext.create('Ext.data.Store', {
    model: 'modeloAutos',
    data: [
        { 'id': 1, 'Marca': 'Audi', 'Linea': 'Coco', 'Modelo': 1999, 'FechaIngreso': new Date((new Date()).setDate( (new Date()).getDate() + 1 )), 'Precio': 245000},
        { 'id': 2, 'Marca': 'GMC', 'Linea': 'Puertas', 'Modelo': 2002, 'FechaIngreso': new Date((new Date()).setDate( (new Date()).getDate() + 6 )), 'Precio': 139050.45},
        { 'id': 3, 'Marca': 'GMC', 'Linea': 'Kilo', 'Modelo': 2005, 'FechaIngreso': new Date((new Date()).setDate( (new Date()).getDate() + 11 )), 'Precio': 660100.99},
        { 'id': 4, 'Marca': 'Tesla', 'Linea': 'Crank', 'Modelo': 1986, 'FechaIngreso': new Date((new Date()).setDate( (new Date()).getDate() + 31 )), 'Precio': 406000},
        { 'id': 5, 'Marca': 'Toyota', 'Linea': 'Humby', 'Modelo': 1995, 'FechaIngreso': new Date((new Date()).setDate( (new Date()).getDate() + 77 )), 'Precio': 390999.99},
        { 'id': 6, 'Marca': 'GMC', 'Linea': 'Dados', 'Modelo': 2016, 'FechaIngreso': new Date((new Date()).setDate( (new Date()).getDate() - 16 )), 'Precio': 275950.65},
        { 'id': 7, 'Marca': 'Audi', 'Linea': 'Sampras', 'Modelo': 2010, 'FechaIngreso': new Date((new Date()).setDate( (new Date()).getDate() - 51 )), 'Precio': 470956.25},
        { 'id': 8, 'Marca': 'Tesla', 'Linea': 'Luna', 'Modelo': 1970, 'FechaIngreso': new Date((new Date()).setDate( (new Date()).getDate() - 111 )), 'Precio': 99666.33},
    ]
});

// Definimos la forma que sera usada tanto
// para AGREGAR como para EDITAR registros,
// NO es necesario crear una forma para
// cada accion
var formaAutos = Ext.create('Ext.form.Panel', {
	title: 'Forma Autos',
	height: 205,
    padding: 12,
	items: [
    	{
			xtype: 'combobox',
            fieldLabel: 'Marca Auto',
			name: 'Marca',
			id: 'cmbMarca',
			store: storeMarcas,
			displayField: 'Marca',
			valueField: 'id'
		},
    	{
			xtype: 'textfield',
            fieldLabel: 'Linea Auto',
			name: 'Linea',
			id: 'txtLineaAuto'
		},
    	{
			xtype: 'numberfield',
            fieldLabel: 'Modelo Auto',
			name: 'Modelo',
			hideTrigger: true,
			id: 'numModeloAuto'
		},
    	{
			xtype: 'datefield',
            fieldLabel: 'Fecha Ingreso',
			name: 'FechaIngreso',
			id: 'dtFechaIngreso',
            // Con la configuracion submitFormat establecida a 'c'
            // el controller de .NET podra recibir la fecha como
            // tipo de dato DateTime. De esta manera siempre
            // manejamos fechas en variables tipo fecha, sin tener
            // que arriesgarnos a hacer conversiones con strings
			submitFormat: 'c'
		},
        {
			xtype: 'numberfield',
            fieldLabel: 'Precio',
			name: 'Precio',
			hideTrigger: true,
			id: 'numPrecioAuto'
		},
	]
});

// Definimos el grid que mostrara a los autos
var gridAutos = Ext.create('Ext.grid.Panel', {
    title: 'Grid Autos',
	height: 215,
    padding: 12,
	store: storeAutos,
	selModel: {
		listeners: {
            // Detectamos la seleccion de un registro del grid mediante el
            // evento 'selectionchange', esto con el proposito de llenar los
            // campos de la forma con la informacion del registro seleccionado
			selectionchange: function(selModel, selection, eOpts) {

                    // Obtenemos la forma
                    var forma = formaAutos.getForm();

                    // Obtenermos la informacion del registro seleccionado
                    var registroInfo = {};
                    if (selection.length > 0)
                        registroInfo = selection[0].data;

                    // Inyectamos la informacion del registro seleccionado
                    // a la forma
                    forma.setValues(registroInfo);
                    
                    // NOTAS:
					// - Los datos del registro se mapean automaticamente a los
					//   campos de la forma, esto es porque lo establecido en la
					//   configuracion 'name' de cada campo de la forma hace
					//   match con lo establecido en la configuracion 'dataIndex'
					//   de cada columna.
					//
                    // - Por lo que NO es necesario mapear individualmente cada
                    //   valor del registro del grid a cada campo de la forma.
                    // 
                    // - De esta manera se pueden llenar las formas sin
					//   importar el origen del objeto de datos, solo con que
					//   los nombres de las propiedades de dicho objeto coinci-
					//   dan con lo establecido en la configuracion 'name' de
					//   los campos de la forma.
			}
		}
    },
	columns: [
        {
			xtype: 'gridcolumn',
			text: 'Marca Auto',
			dataIndex: 'Marca'
		},
        {
			xtype: 'gridcolumn',
			text: 'Linea Auto',
			dataIndex: 'Linea'
		},
        {
			xtype: 'numbercolumn',
			text: 'Modelo Auto',
			dataIndex: 'Modelo',
			format: '0' // Mostrar la cifra como entero
		},
        {
			xtype: 'datecolumn',
			text: 'Fecha Ingreso',
			dataIndex: 'FechaIngreso',
			// Con la configuracion 'format' establecemos
            // la manera de mostrar la fecha en la columna.
            // En este caso 20/11/2019
            format: 'd/m/Y'
		},
        {
			xtype: 'numbercolumn',
			text: 'Precio',
			dataIndex: 'Precio',
            format: '$0,000.00' // Mostrar la cifra con formato de dinero,
                                // separador de coma a 3 pociciones
                                // y mostrando solo 2 decimales
        }
        // NOTA:
        // - Aqui observamos el uso de 3 tipos de columnas,
        //   'gridcolumn' que es un tipo de columna generico,
        //   generalmente utilizado para texto; 'datecolumn',
        //   utilizado para mostrar fechas y 'numbercolumn',
        //   utilizado para mostrar numeros
	]
}).show();

// Definimos la ventana que mostrara
// al grid y a la forma
Ext.create('Ext.window.Window', {
	width: 570,
	height: 478,
    resizable: false,
    header: false,
	items: [
		gridAutos,
		formaAutos
	],
	dockedItems: {
		xtype: 'container',
        layout: {
            type: 'hbox',
            pack: 'center',
            align: 'middle'
        },
        padding: 12,
		dock: 'bottom',
		items: [
            {
				xtype: 'button',
				text: 'Agregar',
                margin: '0 12 0 0',
				handler: function(btn, e) {

                    // Obtenemos la forma
                    var forma = formaAutos.getForm();
                    
                    // Verificamos si cumple con las reglas de sus campos
					if (forma.isValid()) {

                        // A cada campo de la forma se le especifico la configuracion 'name'.
						// Al momento de darle submit a la forma, se genera un objeto con los
						// valores capturados en sus campos, cada valor es asignado a una pro-
                        // piedad de este objeto, el nombre de cada propiedad de dicho objeto
                        // corresponde al valor de la configuracion 'name' de cada campo.
                        //
						// Este objeto sera mandado al backend al dar submit.
						//
						// Del lado del backend debera existir un modelo/clase que tenga las
                        // mismas propiedades, con el mismo nombre y del mismo tipo (equival-
                        // ente) que las del objeto generado por la forma para que el mapeo
                        // de informacion sea exitoso.
						//
                        // Por lo que NO es necesario establecer cada valor de la forma uno
                        // por uno en los parametros de la llamada del submit.

                        // Hacemos submit de la forma
						forma.submit({
							url: 'AgregarAuto',
							// Configuracion que especifica que la informacion de la forma
							// se mandara en forma de objeto
                            jsonSubmit: true,
                            
                            // Acciones de haber sido exitoso el submit
                            success: function(form, action) {
								var respuesta = Ext.decode(action.response.responseText);
                                Ext.Msg.alert('Exito', respuesta.mensaje);
                            },

                            // Acciones de NO haber sido exitoso el submit
                            failure: function(form, action) {
                                switch (action.failureType) {
                                    case Ext.form.action.Action.CLIENT_INVALID:
                                        Ext.Msg.alert('Error', 'No es posible enviar la informacion con valores invalidos');
                                        break;
                                    case Ext.form.action.Action.CONNECT_FAILURE:
                                        Ext.Msg.alert('Error', 'No fue posible establecer conexion');
                                        break;
                                    case Ext.form.action.Action.SERVER_INVALID:
                                       Ext.Msg.alert('Error', action.result.msg);
                               }
                            }
                        });

						// En caso de necesitar el objeto generado por la forma,
						// la podemos obtener de la siguiente manera
						var valoresForma = forma.getValues();
						console.log('valoresForma', valoresForma);
                    }
				}
			},
            {
				xtype: 'button',
				text: 'Editar',
                margin: '0 12 0 0',
				handler: function(btn, e) {
                	var forma = formaAutos.getForm();
					if (forma.isValid()) {
                        
                        // ***************************************************
                        // Aplica la misma logica que en la accion de agregar
                        // ***************************************************
						forma.submit({
							url: 'EditarAuto',
							jsonSubmit: true,
                            success: function(form, action) {
								var respuesta = Ext.decode(action.response.responseText);
                                Ext.Msg.alert('Exito', respuesta.mensaje);
                            },
                            failure: function(form, action) {
                                switch (action.failureType) {
                                    case Ext.form.action.Action.CLIENT_INVALID:
                                        Ext.Msg.alert('Error', 'No es posible enviar la informacion con valores invalidos');
                                        break;
                                    case Ext.form.action.Action.CONNECT_FAILURE:
                                        Ext.Msg.alert('Error', 'No fue posible establecer conexion');
                                        break;
                                    case Ext.form.action.Action.SERVER_INVALID:
                                       Ext.Msg.alert('Error', action.result.msg);
                               }
                            }
                        });

                    }
				}
			},
            {
				xtype: 'button',
				text: 'Limpiar Forma',
                margin: '0 12 0 0',
				handler: function(btn, e) {
                    Ext.Msg.show({
                        title: "Notificacion",
                        msg: "Se perderan la informacion capturada, ¿continuar?",
                        buttons: Ext.Msg.YESNO,
                        fn: function(btn) {
                            if (btn == 'yes') {
                                // Obtenemos la forma
                                var forma = formaAutos.getForm();

                                // Reseteamos la forma (se limpian sus campos)
                                forma.reset();
                            }
                        },
                        icon: Ext.Msg.WARNING
                    });

				}
            },
            {
				xtype: 'button',
				text: 'Cerrar',
				handler: function(btn, e) {
                    // Se obtiene la referencia de la ventana
                    var window = btn.up('window');
                    
                    // Se cierra la ventana
                    window.close();
				}
            }
		]
	}
}).show();
```
:ok_hand:
