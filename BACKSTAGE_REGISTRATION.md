# Backstage Registration Guide

This document explains how to register My Microservice in Backstage.

## 📋 Registration Checklist

### ✅ Files Ready for Backstage

- [x] `catalog-info.yaml` - Complete with all entities
- [x] `openapi.yaml` - Full API specification
- [x] `mkdocs.yml` - TechDocs configuration
- [x] `docs/` - Complete documentation
- [x] `README.md` - Repository overview

### 🎯 Entities Included

1. **System**: `user-management-system`
   - Owner: `group:default/platform-team`
   - Domain: `user-services`

2. **Component**: `my-microservice`
   - Type: `service`
   - Lifecycle: `production`
   - System: `user-management-system`
   - Provides APIs: `my-microservice-api`
   - Depends on: `redis-cache`, `postgres-database`

3. **API**: `my-microservice-api`
   - Type: `openapi`
   - Definition: External file `./openapi.yaml`
   - Complete REST API specification

4. **Resources**:
   - `redis-cache` (cache type)
   - `postgres-database` (database type)

5. **External API**: `external-auth-api`
   - Authentication service integration

## 🚀 Registration Steps

### Option 1: GitHub Repository Registration

1. **Push to GitHub:**
   ```bash
   git init
   git add .
   git commit -m "Initial commit: My Microservice for Backstage"
   git remote add origin https://github.com/your-org/my-microservice.git
   git push -u origin main
   ```

2. **Register in Backstage:**
   - Go to Backstage → Create → Register Existing Component
   - Enter URL: `https://github.com/your-org/my-microservice/blob/main/catalog-info.yaml`
   - Click "Analyze" and "Import"

### Option 2: Manual Import

1. **Copy catalog-info.yaml content**
2. **Go to Backstage → Create → Register Existing Component**
3. **Select "Manual Import"**
4. **Paste the YAML content**
5. **Click "Import"**

## 🔗 Expected Backstage Integration

### Service Catalog

After registration, you'll see:

- **Component**: `my-microservice` in the catalog
- **System**: `user-management-system` with all components
- **APIs**: `my-microservice-api` with OpenAPI spec
- **Resources**: `redis-cache` and `postgres-database`

### TechDocs

Documentation will be available at:
- `https://backstage.example.com/docs/default/component/my-microservice`

### API Documentation

API spec will be available at:
- `https://backstage.example.com/api-docs/default/api/my-microservice-api`

## 📊 Backstage Features Enabled

### ✅ Service Discovery
- Component visible in catalog
- Searchable by tags and metadata
- Owner and team information

### ✅ API Documentation
- Interactive OpenAPI documentation
- API explorer interface
- Request/response examples

### ✅ TechDocs
- Complete documentation rendered
- Navigation and search
- Markdown with Mermaid diagrams

### ✅ Dependency Mapping
- Visual dependency graph
- Resource relationships
- System architecture view

### ✅ Links and Metadata
- GitHub repository link
- Production/staging API links
- Health check endpoints
- Monitoring dashboards

## 🏷️ Tags and Labels

The service will be discoverable by these tags:
- `nodejs`
- `express`
- `microservice`
- `api`
- `rest`
- `user-management`

## 🔄 Updating the Catalog

### Automatic Updates (GitHub Integration)

If using GitHub integration, Backstage will automatically:
- Sync changes from the repository
- Update entity information
- Refresh documentation

### Manual Updates

For manual updates:
1. Update `catalog-info.yaml` in the repository
2. Go to Backstage → Component → Refresh
3. Changes will be reflected immediately

## 🎯 Post-Registration Tasks

### 1. Verify Registration

- [ ] Component appears in catalog
- [ ] API documentation is accessible
- [ ] TechDocs renders correctly
- [ ] Dependencies are linked
- [ ] Links work correctly

### 2. Team Onboarding

- [ ] Share Backstage links with team
- [ ] Update team documentation
- [ ] Add to team dashboards

### 3. Monitoring Integration

- [ ] Add monitoring links to catalog
- [ ] Configure alerting
- [ ] Set up dashboards

## 🔧 Customization Options

### Adding More Links

```yaml
metadata:
  links:
    - url: https://grafana.example.com/d/my-microservice
      title: Grafana Dashboard
      icon: dashboard
    - url: https://sentry.io/my-microservice
      title: Error Tracking
      icon: alert
```

### Additional Annotations

```yaml
metadata:
  annotations:
    prometheus.io/scrape: 'true'
    prometheus.io/port: '3000'
    prometheus.io/path: '/metrics'
```

### Custom Tags

```yaml
metadata:
  tags:
    - production
    - critical
    - user-facing
    - high-availability
```

## 🆘 Troubleshooting

### Common Issues

1. **Entity not appearing**: Check YAML syntax and file accessibility
2. **TechDocs not rendering**: Verify mkdocs.yml configuration
3. **API docs not loading**: Check OpenAPI specification validity
4. **Links not working**: Verify URL accessibility

### Validation

Use these tools to validate before registration:

```bash
# Validate YAML syntax
yamllint catalog-info.yaml

# Validate OpenAPI spec
swagger-codegen validate -i openapi.yaml

# Test mkdocs build
mkdocs build
```

## 📞 Support

For registration issues:
- **Backstage Team**: backstage-support@company.com
- **Platform Team**: platform-team@company.com
- **Documentation**: [Backstage Docs](https://backstage.example.com/docs)

---

**Ready for Backstage registration!** 🚀