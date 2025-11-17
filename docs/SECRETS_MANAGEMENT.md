# Secrets Management Strategy

This document outlines the strategy for managing secrets, credentials, and sensitive configuration data in the Post Ou Cancer platform.

## Principles

1. **Never commit secrets to version control**
2. **Use secure secret storage solutions**
3. **Rotate secrets regularly**
4. **Principle of least privilege**
5. **Audit secret access**
6. **Encrypt secrets at rest and in transit**

## Secret Categories

### 1. Application Secrets

- Database connection strings
- API keys (third-party services)
- JWT signing keys
- Encryption keys
- Service account credentials

### 2. Infrastructure Secrets

- Cloud provider credentials
- Container registry credentials
- Kubernetes secrets
- SSL/TLS certificates
- SSH keys

### 3. User Secrets

- OAuth client secrets (Keycloak, social logins)
- Email service credentials
- Payment gateway keys
- External API keys

## Secret Storage Solutions

### Development Environment

**Status**: ⚠️ To be implemented

- **User Secrets** (`.NET User Secrets`):
  - Use `dotnet user-secrets` for local development
  - Per-user, per-project secrets
  - Not committed to version control
  - File location: `~/.microsoft/usersecrets/<UserSecretsId>/secrets.json`

**Configuration**:

```bash
# Set a secret
dotnet user-secrets set "DatabaseOptions:Password" "dev-password"

# List secrets
dotnet user-secrets list

# Remove a secret
dotnet user-secrets remove "DatabaseOptions:Password"
```

- **Environment Variables**:
  - Use for local development
  - Set in shell or `.env` file (not committed)
  - Example: `export DATABASE_PASSWORD=dev-password`

- **Docker Compose Secrets**:
  - Use Docker secrets for containerized development
  - Mount secrets as files in containers
  - Example: `docker-compose.yml` with `secrets:` section

### Staging/Production Environment

**Status**: ⚠️ To be implemented

**Recommended Solutions**:

1. **Azure Key Vault** (Recommended for Azure deployments)
   - Centralized secret storage
   - Access control via Azure AD
   - Automatic rotation support
   - Audit logging
   - Integration with .NET via `Azure.Extensions.AspNetCore.Configuration.Secrets`

2. **HashiCorp Vault** (Recommended for on-premises/Kubernetes)
   - Open-source secret management
   - Dynamic secrets
   - Secret rotation
   - Kubernetes integration
   - Audit logging

3. **AWS Secrets Manager** (For AWS deployments)
   - Managed secret storage
   - Automatic rotation
   - Integration with AWS services

4. **Kubernetes Secrets** (For Kubernetes deployments)
   - Native Kubernetes secret management
   - Encrypted at rest (with encryption at rest enabled)
   - Mounted as volumes or environment variables
   - **Note**: Base64 encoded, not encrypted by default

## Implementation Strategy

### .NET Configuration

**Status**: ⚠️ To be implemented

Use .NET Configuration with secret providers:

```csharp
// Program.cs
builder.Configuration
    .AddJsonFile("appsettings.json")
    .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true)
    .AddUserSecrets<Program>() // Development only
    .AddEnvironmentVariables()
    .AddAzureKeyVault(/* ... */); // Production
```

### Secret Rotation

**Status**: ⚠️ To be implemented

- **Database passwords**: Rotate every 90 days
- **API keys**: Rotate every 180 days or on compromise
- **JWT signing keys**: Rotate every 90 days (with key rollover period)
- **SSL certificates**: Renew before expiration (automated via Let's Encrypt)

**Rotation Process**:

1. Generate new secret
2. Update secret in secret store
3. Update application configuration (with zero downtime)
4. Verify new secret works
5. Revoke old secret after grace period

### Secret Access Control

**Status**: ⚠️ To be implemented

- **Role-Based Access Control (RBAC)**:
  - Developers: Read-only access to dev secrets
  - DevOps: Full access to infrastructure secrets
  - Applications: Service principal with minimal required permissions

- **Audit Logging**:
  - Log all secret access
  - Alert on unusual access patterns
  - Regular access reviews

## Secret Naming Conventions

**Format**: `{Environment}/{Service}/{SecretName}`

**Examples**:

- `dev/database/postgres-password`
- `prod/api/jwt-signing-key`
- `staging/redis/connection-string`

## Environment-Specific Configuration

### Development

```json
// appsettings.Development.json (committed, no secrets)
{
  "DatabaseOptions": {
    "Host": "localhost",
    "Port": 5432,
    "DatabaseName": "postoucancer",
    "User": "postgres"
    // Password from user secrets or environment variable
  }
}
```

### Production

- No secrets in `appsettings.Production.json`
- All secrets from Azure Key Vault or similar
- Environment variables for non-sensitive configuration

## Secret Injection in Containers

### Docker Compose

```yaml
services:
  api:
    secrets:
      - db_password
    environment:
      - DATABASE_PASSWORD_FILE=/run/secrets/db_password

secrets:
  db_password:
    file: ./secrets/db_password.txt  # Not committed
```

### Kubernetes

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: database-secret
type: Opaque
stringData:
  password: <base64-encoded-password>
---
apiVersion: apps/v1
kind: Deployment
spec:
  template:
    spec:
      containers:
      - name: api
        env:
        - name: DATABASE_PASSWORD
          valueFrom:
            secretKeyRef:
              name: database-secret
              key: password
```

## CI/CD Pipeline Secrets

**Status**: ⚠️ To be implemented

- **GitHub Actions**: Use GitHub Secrets
- **Azure DevOps**: Use Azure Key Vault or Variable Groups
- **GitLab CI**: Use GitLab CI/CD Variables (masked)
- **Never log secrets in build outputs**

## Secret Scanning

**Status**: ⚠️ To be implemented

- **Pre-commit hooks**: Scan for secrets before commit
- **CI/CD pipeline**: Automated secret scanning
- **Tools**:
  - `git-secrets` (AWS)
  - `truffleHog` (general)
  - `gitleaks` (general)
  - GitHub Advanced Security (secret scanning)

## Emergency Procedures

### Secret Compromise

1. **Immediate Actions**:
   - Rotate compromised secret immediately
   - Revoke access for compromised secret
   - Assess impact and scope

2. **Investigation**:
   - Review audit logs
   - Identify how secret was compromised
   - Check for unauthorized access

3. **Remediation**:
   - Rotate all related secrets
   - Update security measures
   - Notify affected parties if required

## Best Practices

1. ✅ **Use secret management services** (Azure Key Vault, Vault, etc.)
2. ✅ **Never hardcode secrets** in code
3. ✅ **Use environment variables** for non-sensitive configuration
4. ✅ **Rotate secrets regularly**
5. ✅ **Use different secrets** for each environment
6. ✅ **Limit secret access** to minimum required
7. ✅ **Audit secret access** regularly
8. ✅ **Encrypt secrets** at rest and in transit
9. ✅ **Use service principals** for application access
10. ✅ **Document secret locations** and access procedures

## Implementation Checklist

- [ ] Set up Azure Key Vault (or alternative) for production
- [ ] Configure .NET User Secrets for development
- [ ] Implement secret injection in containers
- [ ] Set up secret rotation procedures
- [ ] Configure secret scanning in CI/CD
- [ ] Document secret locations and access
- [ ] Set up audit logging for secret access
- [ ] Create emergency procedures for secret compromise
- [ ] Train team on secret management practices

## References

- [.NET User Secrets](https://learn.microsoft.com/en-us/aspnet/core/security/app-secrets)
- [Azure Key Vault](https://azure.microsoft.com/en-us/products/key-vault)
- [HashiCorp Vault](https://www.vaultproject.io/)
- [OWASP Secrets Management](https://cheatsheetseries.owasp.org/cheatsheets/Secrets_Management_Cheat_Sheet.html)
