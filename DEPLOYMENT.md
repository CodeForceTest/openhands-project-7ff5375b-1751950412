# Deployment Guide

## Local Development

To run the website locally:

1. **Simple HTTP Server (Python)**:
   ```bash
   python3 -m http.server 3000
   ```
   Then visit: `http://localhost:3000`

2. **Node.js HTTP Server**:
   ```bash
   npx http-server -p 3000
   ```

3. **PHP Built-in Server**:
   ```bash
   php -S localhost:3000
   ```

## Production Deployment

### Static Hosting Services

#### 1. Netlify
1. Connect your GitHub repository
2. Set build command: (none needed)
3. Set publish directory: `/`
4. Deploy automatically on push

#### 2. Vercel
1. Import GitHub repository
2. Framework preset: Other
3. Build command: (none needed)
4. Output directory: `./`

#### 3. GitHub Pages
1. Go to repository Settings
2. Navigate to Pages section
3. Select source: Deploy from a branch
4. Choose branch: `main`
5. Folder: `/ (root)`

#### 4. Firebase Hosting
```bash
npm install -g firebase-tools
firebase login
firebase init hosting
firebase deploy
```

### Traditional Web Hosting

1. **Upload files via FTP/SFTP**:
   - Upload all files to your web server's public directory
   - Ensure `index.html` is in the root directory

2. **Apache/Nginx Configuration**:
   - No special configuration needed
   - Ensure static file serving is enabled

## Environment Variables

No environment variables are required for this static website.

## Performance Optimization

### For Production:

1. **Minify CSS and JavaScript**:
   ```bash
   # Install minification tools
   npm install -g clean-css-cli uglify-js html-minifier

   # Minify files
   cleancss -o styles.min.css styles.css
   uglifyjs script.js -o script.min.js
   html-minifier --collapse-whitespace --remove-comments index.html -o index.min.html
   ```

2. **Enable Gzip Compression** (server configuration):
   ```apache
   # Apache .htaccess
   <IfModule mod_deflate.c>
       AddOutputFilterByType DEFLATE text/plain
       AddOutputFilterByType DEFLATE text/html
       AddOutputFilterByType DEFLATE text/xml
       AddOutputFilterByType DEFLATE text/css
       AddOutputFilterByType DEFLATE application/xml
       AddOutputFilterByType DEFLATE application/xhtml+xml
       AddOutputFilterByType DEFLATE application/rss+xml
       AddOutputFilterByType DEFLATE application/javascript
       AddOutputFilterByType DEFLATE application/x-javascript
   </IfModule>
   ```

3. **Set Cache Headers**:
   ```apache
   # Apache .htaccess
   <IfModule mod_expires.c>
       ExpiresActive on
       ExpiresByType text/css "access plus 1 year"
       ExpiresByType application/javascript "access plus 1 year"
       ExpiresByType image/png "access plus 1 year"
       ExpiresByType image/jpg "access plus 1 year"
       ExpiresByType image/jpeg "access plus 1 year"
   </IfModule>
   ```

## CDN Integration

### Using CDN for External Resources:

The website already uses CDN for:
- Font Awesome icons
- Google Fonts
- These are loaded from their respective CDNs for optimal performance

### Custom CDN Setup:

1. Upload static assets to a CDN service (CloudFlare, AWS CloudFront, etc.)
2. Update asset URLs in HTML/CSS files
3. Configure proper cache headers

## SSL/HTTPS

Most modern hosting services provide SSL certificates automatically. For custom servers:

1. **Let's Encrypt** (free):
   ```bash
   certbot --nginx -d yourdomain.com
   ```

2. **Commercial SSL**: Purchase and install according to your hosting provider's instructions

## Domain Configuration

1. **DNS Setup**:
   - Point A record to your server's IP address
   - Or CNAME to your hosting service

2. **Subdomain Setup**:
   - Create CNAME record pointing to main domain or hosting service

## Monitoring and Analytics

### Google Analytics Integration:

Add to `<head>` section of `index.html`:

```html
<!-- Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_TRACKING_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'GA_TRACKING_ID');
</script>
```

### Performance Monitoring:

- Google PageSpeed Insights
- GTmetrix
- Pingdom
- WebPageTest

## Security Considerations

1. **Content Security Policy** (CSP):
   ```html
   <meta http-equiv="Content-Security-Policy" content="default-src 'self'; style-src 'self' 'unsafe-inline' https://fonts.googleapis.com https://cdnjs.cloudflare.com; font-src 'self' https://fonts.gstatic.com https://cdnjs.cloudflare.com; script-src 'self';">
   ```

2. **Security Headers**:
   ```apache
   # Apache .htaccess
   Header always set X-Frame-Options DENY
   Header always set X-Content-Type-Options nosniff
   Header always set Referrer-Policy strict-origin-when-cross-origin
   ```

## Backup Strategy

1. **Repository Backup**: GitHub automatically provides backup
2. **Hosting Backup**: Most hosting services provide automatic backups
3. **Manual Backup**: Regularly download site files

## Troubleshooting

### Common Issues:

1. **Fonts not loading**: Check CORS headers for font files
2. **Icons not displaying**: Verify Font Awesome CDN link
3. **Mobile layout issues**: Test responsive breakpoints
4. **Form not working**: Implement backend form handling for production

### Debug Mode:

Add to JavaScript for debugging:
```javascript
// Add to script.js for debugging
console.log('Website loaded successfully');
```

## Maintenance

### Regular Tasks:

1. **Update Dependencies**: Check for Font Awesome and Google Fonts updates
2. **Security Updates**: Monitor for any security advisories
3. **Performance Review**: Regular performance audits
4. **Content Updates**: Keep testimonials and company information current
5. **Backup Verification**: Ensure backups are working properly

### Version Control:

- Use semantic versioning for releases
- Tag important versions in Git
- Maintain changelog for updates