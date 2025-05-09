
# Sincronización Automática de Usuarios y Grupos desde Microsoft Entra ID a Azure Databricks


## Introducción

La **Administración Automática de Identidades** (_Automatic Identity Management_) es una funcionalidad avanzada (actualmente en **versión preliminar pública**) que permite la sincronización integral de usuarios, grupos y entidades de servicio desde **Microsoft Entra ID (Azure Active Directory)** hacia **Azure Databricks**. Esta integración elimina la necesidad de configurar conectores SCIM manuales, posicionando a Entra ID como la fuente única de la verdad para la gestión de identidades en la plataforma de datos.

Con esta característica habilitada, cualquier alta, baja o cambio de pertenencia en Entra ID se replica automáticamente en Databricks, simplificando la administración de acceso y garantizando la coherencia y seguridad en la gestión de identidades.

> **Referencia oficial:**  
> [Automatic Identity Management in Azure Databricks](https://learn.microsoft.com/en-us/azure/databricks/admin/users-groups/automatic-identity-management#enable-automatic-identity-management)


## Requisitos Previos: Unity Catalog e Identidad Federada

Para habilitar la sincronización automática, es imprescindible que el **workspace de Azure Databricks** tenga activada la **federación de identidades**, lo cual está directamente vinculado a la habilitación de **Unity Catalog**. La federación de identidades permite gestionar usuarios y grupos a nivel de cuenta, facilitando la asignación centralizada y la administración multi-workspace.

### Guía paso a paso

1. **Verifica la asociación al metastore** desde el portal de administración:
   ![](image/Pasted%20image%2020250429142331.png)
2. Si el workspace no está asociado:
   ![](image/Pasted%20image%2020250429142534.png)
   Accede a la consola de cuenta y realiza una de las siguientes acciones:
   - Habilita Unity Catalog siguiendo la documentación oficial:  
     [Enable a workspace for Unity Catalog](https://learn.microsoft.com/en-us/azure/databricks/data-governance/unity-catalog/enable-workspaces#requirements)
   ![](image/Pasted%20image%2020250429142740.png)

#### Opción 1: Usar un metastore existente

   - Haz clic en el metastore y verifica que el flag esté habilitado:
     ![](image/Pasted%20image%2020250429142853.png)
   - Asigna el workspace:
     ![](image/Pasted%20image%2020250429142904.png)
     Selecciona el metastore deseado y asígnalo:
     ![](image/Pasted%20image%2020250429142913.png)
   - Confirma que la federación esté habilitada y el metastore asignado:
     ![](image/Pasted%20image%2020250429142929.png)

#### Opción 2: Crear un metastore nuevo

   Si necesitas crear un nuevo metastore, sigue la documentación oficial:
   [Create a Unity Catalog metastore](https://learn.microsoft.com/en-us/azure/databricks/data-governance/unity-catalog/metastore-create)

> **Referencia adicional:**  
> [Unity Catalog Overview](https://learn.microsoft.com/en-us/azure/databricks/data-governance/unity-catalog/)


## Activación de la Sincronización Automática

Para activar la funcionalidad:

1. Accede a la consola de cuenta de Databricks y habilita la preview desde el menú **Previews**:
   ![](image/Pasted%20image%2020250429143323.png)
   Habilita el flag correspondiente:

   ![](image/Pasted%20image%2020250429143351.png)
2. Espera unos minutos a que se complete la sincronización:
   ![](image/Pasted%20image%2020250429143401.png)
3. Verifica el resultado en las pestañas “Users”, “Groups” y “Service Principals”:
   ![](image/Pasted%20image%2020250429143413.png)


## Gestión de usuarios, grupos y service principals

Una vez completados los pasos previos, tendrás la configuración lista y los usuarios, grupos y service principals de Entra ID estarán disponibles en Databricks. A continuación, se detalla una guía paso a paso, ilustrada con imágenes, para cada escenario:

### Usuarios

#### Añadir usuario Entra ID (External)

1. Accede al panel de usuarios desde **User Management** en Databricks y selecciona la opción para añadir usuario:
   ![](image/Pasted%20image%2020250429164834.png)
2. Los usuarios de Entra ID deberían aparecer listados:
   ![](image/Pasted%20image%2020250429164911.png)
3. Una vez agregados, puedes ajustar sus permisos en Databricks:
   ![](image/Pasted%20image%2020250429164932.png)

> [Add users to your account](https://learn.microsoft.com/en-us/azure/databricks/admin/users-groups/users#add-user-account)

#### Añadir usuario Databricks

1. Si necesitas crear un usuario específico para Databricks (no recomendado salvo casos excepcionales), selecciona **"+Create new user:"** y define el nombre:
   ![](image/Pasted%20image%2020250429165257.png)
2. El usuario aparecerá como activo y constará como usuario Databricks:
   ![](image/Pasted%20image%2020250429165305.png)

### Service Principals

#### Añadir Service Principal Entra ID (External)

1. Accede al panel de Service Principals desde **User Management** y selecciona la opción para añadir:
   ![](image/Pasted%20image%2020250429165706.png)
2. Escribe el nombre del service principal de Entra ID (por ejemplo, 'pssql-backend') y selecciónalo:
   ![](image/Pasted%20image%2020250429165741.png)
   ![](image/Pasted%20image%2020250429165804.png)
3. Una vez agregado, estará operativo:
   ![](image/Pasted%20image%2020250429165819.png)

> [Add service principals to your account](https://learn.microsoft.com/en-us/azure/databricks/admin/users-groups/manage-service-principals#add-sp)

#### Añadir Service Principal Databricks

1. Para crear un Service Principal específico de Databricks, selecciona **"Create new service principal:"**:
   ![](image/Pasted%20image%2020250429165826.png)
2. El service principal aparecerá en la lista y en la columna "source" verás "Databricks":
   ![](image/Pasted%20image%2020250429165837.png)

### Grupos

#### Añadir Grupo Entra ID (External)

1. Accede al panel de grupos desde **User Management** y selecciona la opción para añadir grupo:
   ![](image/Pasted%20image%2020250429165927.png)
2. Busca y selecciona el grupo de Entra ID (por ejemplo, "PurviewSecurity"):
   ![](image/Pasted%20image%2020250429165943.png)
3. El grupo aparecerá listado:

   ![](image/Pasted%20image%2020250429170000.png)
4. Podrás ver los componentes del grupo y su correspondencia con Entra ID:
   ![](image/Pasted%20image%2020250429170007.png)
   ![](image/Pasted%20image%2020250429170029.png)
5. Si eliminas un usuario del grupo en Entra ID, el cambio se refleja automáticamente en Databricks:
   ![](image/Pasted%20image%2020250429170042.png)
6. La sincronización es completa y puedes gestionar los roles de los usuarios Entra ID dentro del grupo:
   ![](image/Pasted%20image%2020250429170049.png)
   ![](image/Pasted%20image%2020250429170107.png)
   ![](image/Pasted%20image%2020250429170121.png)

> [Add groups to your account](https://learn.microsoft.com/en-us/azure/databricks/admin/users-groups/manage-groups#add-group)

#### Añadir Grupo Databricks

1. Para crear un grupo específico de Databricks, selecciona "Create new group:":
   ![](image/Pasted%20image%2020250429170157.png)
2. Estos grupos pueden incluir usuarios, service principals u otros grupos, tanto de Entra ID como de Databricks, permitiendo una gestión flexible:
   ![](image/Pasted%20image%2020250429170204.png)
   ![](image/Pasted%20image%2020250429170209.png)
3. En la vista de grupos, puedes identificar el origen (Entra ID o Databricks):
   ![](image/Pasted%20image%2020250429170224.png)


## Eliminación de usuarios

Cuando un usuario es eliminado de Entra ID (por ejemplo, por baja en la compañía), Databricks lo marca como “Account” para evitar la pérdida inmediata de activos asociados. Se recomienda:

1. Crear el usuario y agregarlo al grupo correspondiente:
   ![](image/Pasted%20image%2020250509125501.png)
2. Añadirlo a la lista de usuarios en Databricks:
   ![](image/Pasted%20image%2020250509125610.png)
3. Comprobar que el usuario forma parte del grupo:
   ![](image/Pasted%20image%2020250509125703.png)
4. Tras eliminarlo de Entra ID, Databricks muestra el usuario como “Account”:
   ![](image/Pasted%20image%2020250509135014.png)
   ![](image/Pasted%20image%2020250509135045.png)
   
   Esto permite revisar y transferir los assets asociados antes de eliminarlo definitivamente.
5. Si decides borrar el usuario de Databricks, recibirás un aviso:

   ![](image/Pasted%20image%2020250509135136.png)
   
   Tras confirmar, el usuario desaparecerá de la lista de usuarios activos.


## Buenas prácticas y consideraciones de seguridad

- **Centraliza la gestión de identidades en Entra ID** para garantizar el cumplimiento de políticas corporativas y auditoría.
- **Utiliza Service Principals** para automatizaciones y recursos compartidos, evitando el uso de cuentas personales.
- **Revisa periódicamente los accesos** y realiza auditorías de pertenencia a grupos y roles.
- **Evita la creación de usuarios/grupos nativos** salvo casos justificados.
- **Implementa el principio de mínimo privilegio** y revisa los permisos asignados a usuarios y grupos.
- **Aprovecha las capacidades de logging y monitorización** de Azure y Databricks para detectar accesos no autorizados o anómalos.
- Antes de eliminar un usuario, evalúa sus assets (notebooks, jobs, dashboards):
  - Si son válidos, transfiérelos a otro compañero.
  - Si no aportan valor, elimínalos.
- Revisa los jobs y dashboards que haya creado para evitar fallos por falta de propietario.

> **Referencias clave:**
> - [Azure Identity Management and access control security best practices](https://learn.microsoft.com/en-us/azure/security/fundamentals/identity-management-best-practices)
> - [Databricks Security Best Practices](https://learn.microsoft.com/en-us/azure/databricks/security/)


## Recursos adicionales

- [Sync users and groups automatically from Microsoft Entra ID](https://learn.microsoft.com/en-us/azure/databricks/admin/users-groups/automatic-identity-management#enable-automatic-identity-management)
- [Enable Unity Catalog](https://learn.microsoft.com/en-us/azure/databricks/data-governance/unity-catalog/get-started)
- [Azure Active Directory documentation](https://learn.microsoft.com/en-us/azure/active-directory/)
- [Azure Databricks documentation](https://learn.microsoft.com/en-us/azure/databricks/)
