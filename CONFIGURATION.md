# Hospital Bed & ICU Allocation System - Configuration Guide

## Configuration Files

### 1. application.properties

Location: `src/main/resources/application.properties`

This file contains all application configuration settings.

#### Database Configuration
```properties
# MySQL Database Connection
db.url=jdbc:mysql://localhost:3306/hospital_db
db.username=root
db.password=password

# Connection Pool Settings (optional)
db.pool.minSize=5
db.pool.maxSize=20
db.pool.timeout=30000
```

**Important**: 
- Update `db.username` and `db.password` with your MySQL credentials
- The database `hospital_db` must exist before running the application
- For production, use a dedicated database user with limited privileges

#### Application Settings
```properties
# Application Information
app.name=Hospital Bed & ICU Allocation System
app.version=1.0.0
app.environment=development

# Window Settings
app.window.width=1200
app.window.height=800
app.window.resizable=true
```

#### Bed Configuration
```properties
# Default Bed Counts by Type
bed.general.count=20
bed.icu.count=10
bed.isolation.count=8

# Ward Configuration
ward.general.name=General Ward A
ward.icu.name=Intensive Care Unit
ward.isolation.name=Isolation Ward
```

#### Logging Settings
```properties
# Logging Configuration
logging.enabled=true
logging.level=INFO
logging.file.path=logs/application.log
logging.audit.enabled=true
```

Logging levels: `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`

#### Report Settings
```properties
# Report Export Settings
report.export.path=reports/
report.csv.delimiter=,
report.csv.encoding=UTF-8
report.date.format=yyyy-MM-dd HH:mm:ss
```

---

## Environment-Specific Configuration

### Development Environment

Create: `application-dev.properties`

```properties
db.url=jdbc:mysql://localhost:3306/hospital_db_dev
db.username=dev_user
db.password=dev_password
logging.level=DEBUG
app.environment=development
```

### Production Environment

Create: `application-prod.properties`

```properties
db.url=jdbc:mysql://production-server:3306/hospital_db
db.username=hospital_app
db.password=secure_production_password
logging.level=WARN
app.environment=production

# Security settings
db.pool.maxSize=50
db.pool.timeout=60000
```

### Testing Environment

Create: `application-test.properties`

```properties
db.url=jdbc:mysql://localhost:3306/hospital_db_test
db.username=test_user
db.password=test_password
logging.level=DEBUG
app.environment=test
```

---

## JVM Configuration

### For Development

Create: `dev-run.bat` (Windows) or `dev-run.sh` (Linux/Mac)

```bash
# Windows (dev-run.bat)
@echo off
set JAVA_OPTS=-Xmx1024m -Xms512m
set JAVAFX_OPTS=--module-path "lib/javafx-sdk/lib" --add-modules javafx.controls,javafx.fxml
java %JAVA_OPTS% %JAVAFX_OPTS% -jar target/hospital-bed-icu-allocation-1.0.0.jar
pause
```

```bash
# Linux/Mac (dev-run.sh)
#!/bin/bash
export JAVA_OPTS="-Xmx1024m -Xms512m"
export JAVAFX_OPTS="--module-path lib/javafx-sdk/lib --add-modules javafx.controls,javafx.fxml"
java $JAVA_OPTS $JAVAFX_OPTS -jar target/hospital-bed-icu-allocation-1.0.0.jar
```

### Memory Settings

Adjust based on your needs:
- `-Xms512m` - Initial heap size (512 MB)
- `-Xmx1024m` - Maximum heap size (1 GB)
- `-XX:MaxMetaspaceSize=256m` - Maximum metaspace size

For large hospitals with many patients:
```bash
-Xms1024m -Xmx2048m -XX:MaxMetaspaceSize=512m
```

---

## MySQL Configuration

### Recommended MySQL Settings

Edit `my.cnf` or `my.ini`:

```ini
[mysqld]
# Connection Settings
max_connections=200
wait_timeout=28800

# Performance Settings
innodb_buffer_pool_size=256M
innodb_log_file_size=64M

# Character Set
character-set-server=utf8mb4
collation-server=utf8mb4_unicode_ci

# Time Zone
default-time-zone='+00:00'
```

### Create Dedicated Database User

```sql
-- Create user
CREATE USER 'hospital_admin'@'localhost' IDENTIFIED BY 'secure_password';

-- Grant privileges
GRANT SELECT, INSERT, UPDATE, DELETE ON hospital_db.* TO 'hospital_admin'@'localhost';

-- Grant procedure execution
GRANT EXECUTE ON hospital_db.* TO 'hospital_admin'@'localhost';

-- Apply changes
FLUSH PRIVILEGES;
```

Update `application.properties`:
```properties
db.username=hospital_admin
db.password=secure_password
```

---

## IDE Configuration

### IntelliJ IDEA

1. **Import Project**
   - File → Open → Select `pom.xml`
   - Import as Maven project

2. **Configure JavaFX**
   - File → Project Structure → Libraries
   - Add JavaFX SDK library

3. **Run Configuration**
   - Run → Edit Configurations
   - Add new Application configuration
   - Main class: `com.hospital.App`
   - VM options: `--module-path "path/to/javafx/lib" --add-modules javafx.controls,javafx.fxml`

### Eclipse

1. **Import Project**
   - File → Import → Maven → Existing Maven Projects
   - Select project directory

2. **Add JavaFX**
   - Right-click project → Build Path → Configure Build Path
   - Add External JARs → Add JavaFX SDK JARs

3. **Run Configuration**
   - Run → Run Configurations
   - Java Application → New
   - Main class: `com.hospital.App`
   - Arguments tab → VM arguments: `--module-path "path/to/javafx/lib" --add-modules javafx.controls,javafx.fxml`

### VS Code

1. **Install Extensions**
   - Java Extension Pack
   - Maven for Java

2. **Create Launch Configuration** (`.vscode/launch.json`)
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "java",
            "name": "Launch Hospital App",
            "request": "launch",
            "mainClass": "com.hospital.App",
            "projectName": "hospital-bed-icu-allocation",
            "vmArgs": "--module-path \"path/to/javafx/lib\" --add-modules javafx.controls,javafx.fxml"
        }
    ]
}
```

---

## Troubleshooting Configuration Issues

### Database Connection Fails

1. Verify MySQL is running:
   ```bash
   # Windows
   net start MySQL80
   
   # Linux
   sudo systemctl status mysql
   ```

2. Test connection:
   ```bash
   mysql -u root -p -h localhost
   ```

3. Check firewall settings
4. Verify credentials in `application.properties`

### JavaFX Module Errors

If you see "JavaFX runtime components are missing":

1. Verify JavaFX is in classpath
2. Add VM arguments:
   ```
   --module-path "path/to/javafx/lib"
   --add-modules javafx.controls,javafx.fxml
   ```

### Out of Memory Errors

Increase heap size:
```bash
java -Xmx2048m -jar application.jar
```

### Port Already in Use

If MySQL port 3306 is busy:
```properties
db.url=jdbc:mysql://localhost:3307/hospital_db
```

---

## Security Best Practices

1. **Never commit sensitive data**
   - Add `application-local.properties` to `.gitignore`
   - Use environment variables for passwords

2. **Use strong passwords**
   - Minimum 12 characters
   - Mix of letters, numbers, symbols

3. **Limit database privileges**
   - Don't use root user in production
   - Grant only necessary permissions

4. **Enable SSL for database**
   ```properties
   db.url=jdbc:mysql://localhost:3306/hospital_db?useSSL=true
   ```

---

## Performance Tuning

### Database Connection Pool
```properties
db.pool.minSize=10
db.pool.maxSize=50
db.pool.timeout=30000
db.pool.idleTimeout=600000
```

### JVM Tuning
```bash
-XX:+UseG1GC
-XX:MaxGCPauseMillis=200
-XX:+DisableExplicitGC
```

### Application Settings
```properties
# Disable audit logging for better performance (not recommended)
logging.audit.enabled=false

# Increase batch size for bulk operations
batch.size=100
```

---

## Backup Configuration

Regular backups are essential:

```bash
# Backup database
mysqldump -u root -p hospital_db > backup_$(date +%Y%m%d).sql

# Backup configuration
cp src/main/resources/application.properties application.properties.backup
```

---

For more help, refer to:
- [BUILD_AND_RUN.md](BUILD_AND_RUN.md)
- [DATABASE_SETUP.md](DATABASE_SETUP.md)
- [README.md](README.md)
