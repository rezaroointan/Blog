# Media Upload

The blog supports uploading media assets (images, etc.) directly to Azure Blob Storage while writing blog posts.

## Configuration

The following settings in `appsettings.json` control media upload functionality:

```json
{
  "ImageStorageProvider": "Azure",
  "ImageStorage": {
    "AuthenticationMode": "Default",
    "ConnectionString": "",
    "ServiceUrl": "", 
    "ContainerName": ""
  }
}
```

| Property                        | Type   | Description                                                                                                          |
| ------------------------------- | ------ | -------------------------------------------------------------------------------------------------------------------- |
| ImageStorageProvider            | string | Currently only supports `Azure` for Azure Blob Storage.                                                             |
| AuthenticationMode | string | Authentication method - either `Default` for Microsoft Entra ID or `ConnectionString` for connection string auth |
| ConnectionString   | string | Azure Storage connection string (only used when AuthenticationMode is `ConnectionString`)                          |
| ServiceUrl         | string | Azure Blob Storage service URL (only used when AuthenticationMode is `Default`)                                    |
| ContainerName      | string | Name of the Azure Storage container to store uploaded files.                                                          |

## Authentication Methods

### Default Authentication 
Uses Microsoft Entra ID (formerly Azure AD) managed identity or default credentials. This requires:

1. Setting up proper RBAC permissions in Azure
2. Configuring the ServiceUrl to point to your storage account
3. No sensitive credentials needed in config

### Connection String Authentication
Uses a storage account connection string for authentication:

1. Get connection string from Azure Portal
2. Add it to the ConnectionString setting
3. No additional Azure setup required

## Usage

1. Start writing a blog post in the markdown editor
2. Drag & drop images onto the editor
3. A dialog appears asking for:
   - File name (can include subdirectories)
   - Whether to enable browser caching via `Cache-Control` headers (currently set to one week)
4. The image gets uploaded and a markdown image link is inserted

## Performance Note

Currently, the blog software serves images directly from Azure Blob Storage. For better performance and scalability, consider:

- Using Azure CDN in front of the storage account
- Replacing the blob storage URLs with CDN URLs
- Benefits include HTTP/2 support and improved caching