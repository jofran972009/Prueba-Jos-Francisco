
# Cómo agregar controller
* Crear controller java
```java
@Controller
@RequestMapping("/nuevoController")
public class NuevoController{}
```
* Registrar el controller en `mvc-dispatcher-servlet.xml`
```xml
<bean id="NuevoController" class="com.overcome.test.controller.NuevoController" />
``` 
* Registrar el `RequestMapping` dentro  `security-app-context.xml` para asignar algún tipo de seguridad
 

# Cómo agregar una nueva entidad
```java
@Entity
public class NuevaEntidad{}
```
* Declarar la entidad con etiquetas @Entity
* Declarar un bean de ObjectifyDao
```xml
<bean id="nuevaEntidadDao" class="com.overcome.dao.ObjectifyDao">
     <constructor-arg name="clazz" value="com.overcome.test.entity.NuevaEntidad" />
</bean>
```
* Registrar la entidad en `mvc-dispatcher-servlet.xml` en bean OfyService como un nuevo elemento del arreglo de clases
```xml
<bean id="OfyService" class="com.overcome.dao.OfyService" factory-method="getInstance">
	<constructor-arg name="classes">
		<array value-type="java.lang.Class">
            ...
			<value>com.overcome.test.entity.NuevaEntidad</value>
		</array>
	</constructor-arg>
</bean>
```

# Cómo agregar un nuevo service
* Crear el service y service implementation 
* Registrar la implementación del service en `mvc-dispatcher-servlet.xml` como un bean
```xml
<bean id="nuevaEntidadService" class="com.overcome.test.service.impl.NuevaEntidadImpl">
     <property name="objectifyDao" ref="nuevaEntidadDao" /> // Es el bean que se declaró al registrar el bean del nueva entidad
</bean>
```
* Agregar en `mvc-dispatcher-servlet.xml` dentro del bean de ServiceFactory una propiedad don el nuevo service
``` xml
<bean id="ServiceFactory" class="com.overcome.test.ServiceFactory" factory-method="getInstance">
	 ...
     <property name="roleService" ref="nuevaEntidadService" />
</bean>
```
* Dentro de ServiceFactory.java declarar el atributo `nuevaEntidadService` y escribir `getters` y `setters`

# Cómo agregar un nuevo functions
* Declarar una nueva clase java que se comportará como el functions deseado
* Declararlo en 
```xml
<bean id="nuevoFunctions" class="com.overcome.test.functions.NuevoFunctions" />
```
* Luego inyectarlo dónde lo necesites, por ejemplo en un `controller``
```xml
<bean id="RolesController" class="com.overcome.test.controller.RolesController">
	<property name="rolesFunctions" ref="rolesFunctions" />
</bean>
```


