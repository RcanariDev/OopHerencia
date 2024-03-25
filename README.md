# Programación orientadad a objetos (OOP)

<br/>
<br/>

## Herencia

- Librerias
```
import pandas as pd
import sqlalchemy
import pyodbc
```

<br/>

- Clase madre
```
class Acceso:
    
    def __init__(self):
        self.cnxn = None
        self.cur = None

    def InicioSesion(self):
        self.cnxn = pyodbc.connect('DRIVER={SQL Server};'
                      'Server=eis-app.database.windows.net;'
                      'Database=eis;'
                      'Port=1433;'
                      'UID=eis-pbi;'
                      'PWD=123ppp+++;')
        self.cur = self.cnxn.cursor()
    
    def CerrarSesion(self):
        
        self.cur.close()
        self.cnxn.close()
```

<br/>

- Clase hija
```
class Upload(Acceso):
    
    def __init__(self):
        super().__init__()
        
    def ConsultarTabla(self):
        super().InicioSesion()
        
        ConsultaTablas = f'''
        select B.Name as NombreTabla, dateadd(hour, -5, A.last_user_update) as FechaUltimaActualizacionAjustada
        from sys.dm_db_index_usage_stats A
        left join sys.tables B on A.object_id = B.object_id
        where B.name = 'DashVentasUnionTab12'
        '''
        Data11 = pd.read_sql(ConsultaTablas, self.cnxn)
        
        super().CerrarSesion()
        
        return Data11
```

<br/>

- Llamar a la clase hija
```
Prueba11 = Upload()
Prueba11.ConsultarTabla()
```



