# Bases-de-datos
_Cómo cargar y consultar información en bases de datos_ :smiley_cat:
<details>
<summary><h2>Cómo crear un blob de almacenamiento</h2></summary>
  <h3> Crear una cuenta de almacenamiento</h3>
  <ol>
  <li> Inicie sesión en Azure Portal en https://portal.azure.com.</li>
  <li> Seleccione Crear un recurso.</li>
  <li>En Categorías, seleccione Almacenamiento.</li>
  <li>En Cuenta de almacenamiento, seleccione Crear.</li>
  <li>En la pestaña Aspectos básicos del panel Crear cuenta de almacenamiento, rellene la siguiente información. Deje los valores predeterminados para todo lo demás.</li>
    <table aria-label="Tabla 1" class="table">
<thead>
<tr>
<th><strong>Configuración</strong></th>
<th><strong>Valor</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td>Subscription</td>
<td>Azure for Students</td>
</tr>
<tr>
<td>Grupo de recursos</td>
<td><rgn data-author-content="[nombre del grupo de recursos del espacio aislado]">[nombre del grupo de recursos del espacio aislado]</rgn></td>
</tr>
<tr>
<td>Nombre de la cuenta de almacenamiento</td>
<td>Creación de un nombre de cuenta de almacenamiento único</td>
</tr>
<tr>
<td>Ubicación</td>
<td>default</td>
</tr>
<tr>
<td>Rendimiento</td>
<td>Estándar</td>
</tr>
<tr>
<td>Redundancia</td>
<td>Almacenamiento con redundancia local (LRS)</td>
</tr>
</tbody>
</table>
    <li>Seleccione Revisar y crear para revisar la configuración de su cuenta de almacenamiento y permitir que Azure valide la configuración.</li>
    <li>Una vez validada, seleccione Crear. Espere la notificación de que la cuenta se creó correctamente.</li>
    <li>Seleccione Ir al recurso.</li>
  </ol>
  
  <h3>Uso con Blob Storage</h3>
  _Creará un contenedor de blobs y cargará un archivo_
  <ol>
    <li>En Almacenamiento de datos, seleccione Contenedores.</li>
    <li>Seleccione Nuevo(+) contenedor, asígnele un nombre y configure un nivel de acceso privado</li>
    <li>Seleccione crear</li>
    <li>Seleccione el contenedor creado y seleccione cargar</li>
    <li>Seleccione el blob (archivo) que acaba de cargar. Debe estar en la pestaña de propiedades.</li>
    <li>Copie la dirección URL del campo URL y péguela en una nueva pestaña. Debe recibir un mensaje de error similar al siguiente:
 <pre><code>&lt;Error&gt;
  &lt;Code&gt;ResourceNotFound&lt;/Code&gt;
  &lt;Message&gt;The specified resource does not exist. RequestId:4a4bd3d9-101e-005a-1a3e-84bd42000000 Time:2022-06-20T00:41:31.2482656Z&lt;/Message&gt;
&lt;/Error&gt;

</code></pre>
</li>
</ol>
  
  <h3>Cambie el nivel de acceso del blob</h3>
  <ol>
    <li>Vuelva a Azure Portal.</li>
    <li>Seleccione Cambiar nivel de acceso.</li>
    <li>Establezca el Nivel de acceso público en Blob (acceso de lectura anónimo solo para blobs).</li>
    <li>Captura de pantalla con el cambio en el nivel de acceso resaltado.</li>
    <li>Seleccione OK (Aceptar).</li>
    <li>Actualice la pestaña en la que ha intentado acceder al archivo anteriormente.</li>
  </ol>
  </details>
 
 <details>
  <summary><h2>Consultar datos con Kutso</h2></summary>
  <i>Kutso nos permite realizar solicitudes de lectura para consultar datos.
  <p>Podemos utilizar los blobs como fuentes de datos para ingerir en un cluster o crear una tabla cargando datos directamente, se ordenan en columnas de acuerdo al tipo de datos y posteriormente se pueden realizar consultas sobre estor datos. En esta ocasión, tomaremos ejemplos de tablas ya creadas.</p></i>
<h2 id="connect-to-the-data">Conexión a los datos</h2>
<p>Usará la interfaz web de Azure Data Explorer para conectarse a los datos. Pero también puede usar el Lenguaje de consulta Kusto mismo en Log Analytics, Azure Sentinel y otros servicios. Solo tendrá que conectarse una vez y seguirá usando esta conexión de datos para todas las consultas de las unidades siguientes.</p>
<ol>
<li><p>Use la cuenta de Azure para iniciar sesión en la <a href="https://dataexplorer.azure.com/" data-linktype="external">interfaz de usuario web de Azure Data Explorer</a>.</p>
</li>
<li><p>En el panel izquierdo, seleccione <strong>Consulta</strong>.</p>
</li>
<li><p>Seleccione el botón <strong>Agregar clúster</strong> en la parte superior de la pestaña.</p>
</li>
<li><p>En el cuadro de diálogo, en <strong>URI de conexión</strong>, escriba <em>help</em>.</p>
</li>
<li><p>Seleccione <strong>Agregar</strong>.</p></li>
</ol>
<p>Ya está conectado al clúster help.</p>
<h3 id="select-the-database">Seleccione la base de datos</h3>
<p>Las consultas siempre se ejecutan en el contexto de una base de datos, por lo que debe conectarse a una específica.</p>
<ol>
<li><p>Expanda el clúster help en el panel izquierdo.</p>
</li>
<li><p>Seleccione la base de datos <strong>Samples</strong> para proporcionar a las consultas el contexto correcto.</p></li>
<li><p>Si expande la base de datos <strong>Samples</strong>, <strong>Tables</strong> y la carpeta <strong>Storm_Events</strong>, verá una lista de tablas debajo de la base de datos; usaremos la tabla <em>StormEvents</em>.</p>
</li>
</ol>
<p>Ya está listo para ejecutar consultas en la tabla <em>StormEvents</em>. </p>
  <h2>Operadores</h2>
  <h4><code>Take</code></h4>
  <p>Devuelve un número específico de filas arbitrarias.</p>
  Puede ejecutar la siguiente consulta:
  <pre tabindex="0" class="has-inner-focus"><code class="lang-kusto" data-author-content="StormEvents
| take 5
"><span>StormEvents
| <span class="hljs-title">take</span> <span class="hljs-number">5</span>
</span></code></pre>
  <h4><code>Project:</code></h4>
  <p>Devuelve sólo las columnas que se seleccionen.</p>
  <pre tabindex="0" class="has-inner-focus"><code class="lang-kusto" data-author-content="StormEvents
| project EventType, State, DamageProperty, DamageCrops, InjuriesDirect, InjuriesIndirect
| take 10
"><span>StormEvents
| <span class="hljs-title">project</span> EventType, State, DamageProperty, DamageCrops, InjuriesDirect, InjuriesIndirect
| <span class="hljs-title">take</span> <span class="hljs-number">10</span>
</span></code></pre>
  <p>Puede definir nuevas columnas sumando valores enteros de distintas columnas, por ejemplo:</p>
  <p><code>Injuries=InjuriesDirect+InjuriesIndirect</code></p>
 Prueba y analiza la siguente consulta:
  <pre tabindex="0" class="has-inner-focus"><code class="lang-kusto" data-author-content="StormEvents
| project US_State=State, EventType, Injuries=InjuriesDirect+InjuriesIndirect, Damage=DamageCrops+DamageProperty
| take 10
"><span>StormEvents
| <span class="hljs-title">project</span> US_State=State, EventType, Injuries=InjuriesDirect+InjuriesIndirect, Damage=DamageCrops+DamageProperty
| <span class="hljs-title">take</span> <span class="hljs-number">10</span>
</span></code></pre>
  También puede utilizar el operador <code>project-away</code> para eliminar columnas:
  <pre tabindex="0" class="has-inner-focus"><code class="lang-kusto" data-author-content="StormEvents
| project-away EpisodeId, EventId
| take 10
"><span>StormEvents
| <span class="hljs-title">project-away</span> EpisodeId, EventId
| <span class="hljs-title">take</span> <span class="hljs-number">10</span>
</span></code></pre>
  <h4><code>Where</code></h4>
  <p>Filtra los resultados que satisfacen una condición determinada.Estas condiones las podemos establecer con operadores numéricos como <i>mayorque</i><code>></code>, valores de cadena como <code>"FLORIDA"</code>, mediante el operador <code>has</code> o con valores datatipe, (que contiene el formato)<code>where</code><em>hora</em><code>between</code><code>(datetime(</code><em>valor</em><code>)..datetime(</code><em>valor</em><code>))</code></p>
  Prueba y analiza la siguiente consulta:
  <pre tabindex="0" class="has-inner-focus"><code class="lang-kusto" data-author-content="StormEvents
| where StartTime between (datetime(2007-01-01)..datetime(2007-06-01))
| where DamageProperty > 0
| where State == &quot;FLORIDA&quot;
| project StartTime, EventType, DamageProperty
| take 50
"><span>StormEvents
| <span class="hljs-title">where</span> StartTime <span class="hljs-keyword">between</span> (<span class="hljs-number">datetime(2007-01-01)</span>..<span class="hljs-number">datetime(2007-06-01)</span>)
| <span class="hljs-title">where</span> DamageProperty &gt; <span class="hljs-number">0</span>
| <span class="hljs-title">where</span> State == <span class="hljs-string">"FLORIDA"</span>
| <span class="hljs-title">project</span> StartTime, EventType, DamageProperty
| <span class="hljs-title">take</span> <span class="hljs-number">50</span>
</span></code></pre>
  
  <h4><code>Sort</code></h4>
  
  <p>El operador sort organiza los datos para que sean más faciles de entender, veamos su aplicación de manera ascendete y descendente</p>
  <pre tabindex="0" class="has-inner-focus"><code class="lang-kusto" data-author-content="StormEvents
| where DamageProperty > 0
| project State, EventType, DamageProperty
| sort by DamageProperty desc
"><span>StormEvents
| <span class="hljs-title">where</span> DamageProperty &gt; <span class="hljs-number">0</span>
| <span class="hljs-title">project</span> State, EventType, DamageProperty
| <span class="hljs-title">sort</span> <span class="hljs-keyword">by</span> DamageProperty <span class="hljs-keyword">desc</span>
</span></code></pre>
  
  <pre tabindex="0" class="has-inner-focus"><code class="lang-kusto" data-author-content="StormEvents
| where DamageProperty > 0
| project State, EventType, DamageProperty
| sort by State asc, DamageProperty
"><span>StormEvents
| <span class="hljs-title">where</span> DamageProperty &gt; <span class="hljs-number">0</span>
| <span class="hljs-title">project</span> State, EventType, DamageProperty
| <span class="hljs-title">sort</span> <span class="hljs-keyword">by</span> State <span class="hljs-keyword">asc</span>, DamageProperty
</span></code></pre>
  
 <p>Dentro de sort, también existe el operador <code>top</code> que muestra un número específico de resultados principales. </p>
  
  <pre tabindex="0" class="has-inner-focus"><code class="lang-kusto" data-author-content="StormEvents
| where State == &quot;FLORIDA&quot;
| top 10 by DamageProperty
| project State, EventType, DamageProperty
"><span>StormEvents
| <span class="hljs-title">where</span> State == <span class="hljs-string">"FLORIDA"</span>
| <span class="hljs-title">top</span> <span class="hljs-number">10</span> <span class="hljs-keyword">by</span> DamageProperty
| <span class="hljs-title">project</span> State, EventType, DamageProperty
</span></code></pre>
 </details>
