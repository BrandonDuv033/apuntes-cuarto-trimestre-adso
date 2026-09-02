# Apuntes ADSO · SENA

![Ficha](https://img.shields.io/badge/Ficha-3315796-2563EB?style=flat-square)
![Jornada](https://img.shields.io/badge/Jornada-Diurna-F59E0B?style=flat-square)
![Trimestre](https://img.shields.io/badge/Trimestre-4-10B981?style=flat-square)

Repositorio de apuntes del Tecnólogo en Análisis y Desarrollo de Software (ADSO) - SENA, ficha **3315796**, jornada diurna, cuarto trimestre.

Cada carpeta corresponde a una materia y contiene su propio índice de clases en orden cronológico (ISO 8601).

## Equipo ejecutor

| Instructor(a) | Correo | Materia a cargo |
|---|---|---|
| Zaida Patricia Ojeda Guzmán | zpatriciao@misena.edu.co | Bases de datos (SQL) |
| Heiver Cuesta Dávila | hcuesta@sena.edu.co | Codificación del software (React) |
| Julio Eduardo Valenzuela Díaz | jevalenzuela@sena.edu.co | Inglés |
| Nubia Marcela Benítez López | nbenitez@sena.edu.co | Protección ambiental y SST |

## Horario semanal

| Día | Instructor(a) | Hora | Ambiente |
|---|---|---|---|
| Lunes | Zaida Patricia Ojeda Guzmán | 6:00 - 10:00 | 304 |
| Lunes | Julio Eduardo Valenzuela Díaz | 10:00 - 14:00 | 319 |
| Martes | Zaida Patricia Ojeda Guzmán | 6:00 - 10:00 | 304 |
| Miércoles | Heiver Cuesta Dávila | 6:00 - 10:00 | 304 |
| Jueves | Heiver Cuesta Dávila | 6:00 - 10:00 | 304 |
| Viernes | Zaida Patricia Ojeda Guzmán | 6:00 - 10:00 | 304 |

## Explora las materias

| | Materia | Instructor(es) |
|---|---|---|
| ![SQL](https://img.shields.io/badge/SQL-Bases%20de%20Datos-4479A1?style=flat-square&logo=mysql&logoColor=white) | [Ver apuntes](./sql/README.md) | Zaida Patricia Ojeda Guzmán |
| ![React](https://img.shields.io/badge/React-Frontend-61DAFB?style=flat-square&logo=react&logoColor=black) | [Ver apuntes](./react/README.md) | Zaida Patricia Ojeda Guzmán · Heiver Cuesta Dávila |
| ![Inglés](https://img.shields.io/badge/Ingles-Comunicacion-B22234?style=flat-square) | [Ver apuntes](./ingles/README.md) | Julio Eduardo Valenzuela Díaz |
| ![Proyecto](https://img.shields.io/badge/Proyecto-Formativo-FF6F00?style=flat-square) | [Ver apuntes](./proyecto/README.md) | Equipo ejecutor |
| ![SST](https://img.shields.io/badge/SST-Ambiental-2E7D32?style=flat-square) | [Ver apuntes](./sst/README.md) | Nubia Marcela Benítez López |

## Estructura del repositorio

```
.
├── README.md
├── sql/
│   └── README.md
├── react/
│   └── README.md
├── ingles/
│   └── README.md
├── proyecto/
│   └── README.md
└── sst/
    └── README.md
```

## Convenciones

- **Nomenclatura de archivos:** `YYYY-MM-DD-nombre-del-tema.md`
- **Commits semánticos:** Conventional Commits adaptados a documentación, ej. `docs(sql): agregar comandos DML y configuración de XAMPP`
- **Frontmatter obligatorio** en cada apunte:
  ```yaml
  ---
  fecha: YYYY-MM-DD
  materia: 
  tema: 
  ---
  ```
