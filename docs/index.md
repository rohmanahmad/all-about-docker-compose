# Welcome to All About Docker Compose

This site is a comprehensive guide to Docker Compose, covering installation, configuration, best practices, troubleshooting, and resources for managing multi-container Docker applications.

# Nginx API Key and Bearer Token Authentication Example

This configuration demonstrates how to secure an Nginx reverse proxy using both X-API-KEY and Authorization: Bearer token headers, with dynamic token management via a map file.

## Example nginx.conf

```nginx
events {}
http {
    # Validate X-API-KEY header
    map $http_x_api_key $api_key_valid {
        default 0;
        include /etc/nginx/api_key.map;
    }

    # Validate Authorization: Bearer <token> header
    map $http_authorization $bearer_token_raw {
        default "";
        ~^Bearer\ (?<bearer>[A-Za-z0-9]+)$ $bearer;
    }

    # Map valid Bearer tokens (same as API key map)
    map $bearer_token_raw $bearer_valid {
        default 0;
        include /etc/nginx/api_key.map;
    }

    server {
        listen 8080;

        location / {
            set $auth_valid 0;
            if ($api_key_valid = 1) {
                set $auth_valid 1;
            }
            if ($bearer_valid = 1) {
                set $auth_valid 1;
            }
            if ($auth_valid = 0) {
                return 401 "Unauthorized: Invalid API Key or Bearer token\n";
            }
            # proxy_pass http://ollama:11434;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
}
```

## Usage
- Add valid API keys or Bearer tokens to `/etc/nginx/api_key.map` in the format:
  ```
  <token> 1;
  ```
- Requests must include either `X-API-KEY` or `Authorization: Bearer <token>` header matching a value in the map file.
