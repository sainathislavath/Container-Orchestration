# --- build stage ---
    FROM node:18-alpine AS build
    WORKDIR /fe
    
    # If you need relaxed peer deps (MUI v4 vs React 18), bake it in:
    ENV NPM_CONFIG_LEGACY_PEER_DEPS=true
    
    # Copy only package manifests first for better caching
    COPY package*.json ./
    
    # Install deps (use install instead of ci to honor legacy peer deps)
    RUN npm install
    
    # Copy the rest of the app
    COPY . .
    
    # CRA build output goes to /fe/build
    # (If you ever switch to Vite, change to `npm run build` and deploy /fe/dist)
    RUN npm run build
    
    # --- run stage ---
    FROM nginx:alpine
    # Remove default Nginx page
    RUN rm -rf /usr/share/nginx/html/*
    
    # Copy CRA build artifacts
    COPY --from=build /fe/build /usr/share/nginx/html
    
    # Expose port 80 for the container
    EXPOSE 80
    
    # Default Nginx startup
    CMD ["nginx", "-g", "daemon off;"]
    