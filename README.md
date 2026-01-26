# Roman Velička - Professional CV/Portfolio Website

A modern, responsive, bilingual (Czech/English) personal portfolio website showcasing professional experience, skills, projects, and contact information.

## 🌟 Features

- **Bilingual Support**: Automatic language detection (Czech for CZ visitors, English for others)
- **Manual Language Toggle**: Easy switching between Czech and English
- **Responsive Design**: Mobile-first approach, works perfectly on all devices
- **Modern UI/UX**: Clean, professional aesthetic with smooth animations
- **SEO Optimized**: Proper meta tags and semantic HTML structure
- **Fast Loading**: Lightweight vanilla JavaScript, no heavy frameworks
- **Smooth Animations**: Subtle entrance animations and transitions
- **Interactive Elements**: Hover effects, smooth scrolling navigation
- **Project Filtering**: Tab-based filtering for different project categories
- **Timeline Layout**: Visual timeline for experience and education
- **Performance Optimized**: Minimal dependencies, optimized assets

## 📁 Project Structure

```
cv-portfolio/
├── index.html              # Main HTML file
├── css/
│   ├── main.css           # Core styles and layout
│   ├── responsive.css     # Mobile and tablet responsive styles
│   └── animations.css     # Animation keyframes and effects
├── js/
│   ├── language.js        # Bilingual content and language detection
│   ├── navigation.js      # Navigation, scroll effects, and filtering
│   └── animations.js      # Scroll animations and visual effects
├── deployment/
│   ├── nginx-deployment.md      # Raspberry Pi deployment guide
│   └── cloudflare-pages.md      # Cloudflare Pages deployment guide
└── README.md              # This file
```

## 🚀 Quick Start

### Local Development

1. **Clone or download the repository**
   ```bash
   git clone https://github.com/yourusername/cv-portfolio.git
   cd cv-portfolio
   ```

2. **Open in browser**
   
   Simply open `index.html` in your web browser:
   - **Double-click** `index.html`, or
   - **Right-click** → Open with → Your browser
   - Or use a local server (recommended):

3. **Using a local server** (recommended for testing):

   **Python 3:**
   ```bash
   python3 -m http.server 8000
   ```
   Then visit: `http://localhost:8000`

   **Python 2:**
   ```bash
   python -m SimpleHTTPServer 8000
   ```

   **Node.js (http-server):**
   ```bash
   npx http-server -p 8000
   ```

   **PHP:**
   ```bash
   php -S localhost:8000
   ```

   **VS Code Live Server Extension:**
   - Install "Live Server" extension
   - Right-click `index.html` → "Open with Live Server"

## 🌍 Language System

The website automatically detects the user's preferred language based on their browser settings:

- **Czech (cs)**: Displayed for users with Czech browser language or location
- **English (en)**: Default for all other users

### How It Works

1. **Automatic Detection**: Checks browser language on page load
2. **LocalStorage**: Saves user's language preference
3. **Manual Toggle**: Users can switch languages using the flag button
4. **Persistent**: Preference is remembered across sessions

### Modifying Content

All text content is stored in `js/language.js` in the `translations` object:

```javascript
const translations = {
    cs: {
        // Czech translations
    },
    en: {
        // English translations
    }
};
```

To modify content:
1. Open `js/language.js`
2. Find the relevant section (e.g., `about.summary`, `experience.work1`)
3. Edit the text in both Czech and English
4. Save and reload the page

## 🎨 Customization

### Colors

Edit CSS custom properties in `css/main.css`:

```css
:root {
    --primary-color: #2563eb;      /* Main brand color */
    --primary-dark: #1e40af;       /* Darker shade */
    --primary-light: #3b82f6;      /* Lighter shade */
    --accent-color: #0ea5e9;       /* Accent color */
    /* ... more color variables */
}
```

### Fonts

The website uses **Inter** font from Google Fonts. To change:

1. Update the Google Fonts link in `index.html`:
   ```html
   <link href="https://fonts.googleapis.com/css2?family=YourFont:wght@300;400;500;600;700&display=swap" rel="stylesheet">
   ```

2. Update the font family in `css/main.css`:
   ```css
   --font-primary: 'YourFont', sans-serif;
   ```

### Profile Image

Replace the SVG placeholder in `index.html` with your image:

```html
<!-- Find this section and replace with: -->
<div class="hero-image-wrapper">
    <img src="assets/images/profile.jpg" alt="Roman Velička" class="hero-image">
</div>
```

Then add this CSS in `css/main.css`:

```css
.hero-image {
    width: 160px;
    height: 160px;
    border-radius: 50%;
    object-fit: cover;
    box-shadow: var(--shadow-lg);
}
```

### Adding New Sections

1. Add HTML in `index.html`:
   ```html
   <section id="new-section" class="section">
       <div class="container">
           <h2 class="section-title" data-i18n="newSection.title">Title</h2>
           <!-- Your content -->
       </div>
   </section>
   ```

2. Add navigation link:
   ```html
   <li class="nav-item">
       <a href="#new-section" class="nav-link" data-i18n="nav.newSection">Link</a>
   </li>
   ```

3. Add translations in `js/language.js`:
   ```javascript
   nav: {
       newSection: "Nový oddíl" // Czech
   },
   newSection: {
       title: "Název sekce" // Czech
   }
   ```

## 📱 Responsive Breakpoints

The website is optimized for these breakpoints:

- **Mobile**: < 768px
- **Tablet**: 768px - 1024px
- **Desktop**: > 1024px
- **Large Desktop**: > 1280px

Responsive styles are in `css/responsive.css`.

## 🔧 Technical Details

### Technologies Used

- **HTML5**: Semantic markup
- **CSS3**: Custom properties, Grid, Flexbox, animations
- **Vanilla JavaScript**: No frameworks or libraries
- **Google Fonts**: Inter font family
- **SVG Icons**: Inline SVG for scalable icons

### Browser Support

- ✅ Chrome/Edge (latest)
- ✅ Firefox (latest)
- ✅ Safari (latest)
- ✅ Mobile browsers (iOS Safari, Chrome Mobile)
- ⚠️ Internet Explorer: Not supported (modern JavaScript required)

### Performance

- **Lightweight**: ~50KB total (HTML + CSS + JS)
- **Fast Loading**: No external dependencies except Google Fonts
- **Optimized**: Minified CSS/JS recommended for production
- **SEO Friendly**: Semantic HTML, proper meta tags

## 📦 Deployment Options

### 1. Raspberry Pi with Nginx

Perfect for self-hosting and learning network administration.

**Pros:**
- Complete control over infrastructure
- Great learning experience
- No hosting costs (after Pi purchase)
- Can integrate with home network projects

**Steps:**
See detailed guide: [`deployment/nginx-deployment.md`](deployment/nginx-deployment.md)

**Quick summary:**
```bash
# Install Nginx
sudo apt install nginx -y

# Copy files to web directory
sudo cp -r * /var/www/cv-portfolio/

# Configure Nginx
sudo nano /etc/nginx/sites-available/cv-portfolio

# Enable and restart
sudo ln -s /etc/nginx/sites-available/cv-portfolio /etc/nginx/sites-enabled/
sudo systemctl reload nginx
```

### 2. Cloudflare Pages

Best for production deployment with global CDN.

**Pros:**
- Free hosting with unlimited bandwidth
- Global CDN (275+ locations)
- Automatic SSL certificates
- Git integration with auto-deployments
- Built-in analytics
- DDoS protection

**Steps:**
See detailed guide: [`deployment/cloudflare-pages.md`](deployment/cloudflare-pages.md)

**Quick summary:**
1. Push code to GitHub
2. Connect repository to Cloudflare Pages
3. Deploy automatically
4. Add custom domain (optional)

### 3. Other Deployment Options

#### Netlify
```bash
# Install Netlify CLI
npm install -g netlify-cli

# Deploy
netlify deploy --prod --dir=.
```

#### Vercel
```bash
# Install Vercel CLI
npm install -g vercel

# Deploy
vercel --prod
```

#### GitHub Pages
1. Push to GitHub
2. Go to repository Settings → Pages
3. Select branch to deploy
4. Your site will be at: `https://username.github.io/cv-portfolio`

#### Traditional Web Hosting
Upload files via FTP/SFTP to any web hosting provider:
- Upload all files to `public_html` or `www` directory
- Ensure `index.html` is in the root
- Set proper file permissions (755 for directories, 644 for files)

## 🔒 Security Best Practices

### For Raspberry Pi Deployment:

1. **Use HTTPS**: Install Let's Encrypt SSL certificate
2. **Firewall**: Enable UFW and allow only necessary ports
3. **SSH Security**: Use key-based authentication, disable password login
4. **Regular Updates**: Keep Raspberry Pi OS and Nginx updated
5. **Fail2Ban**: Install to prevent brute force attacks
6. **Backup**: Regular backups of website and configuration

### For All Deployments:

1. **Security Headers**: Added via `_headers` file or server config
2. **No Sensitive Data**: Don't include API keys or passwords in code
3. **Input Validation**: (If you add forms later)
4. **Regular Updates**: Keep dependencies updated

## 🎯 SEO Optimization

The website includes:

- ✅ Semantic HTML5 structure
- ✅ Meta description and keywords
- ✅ Open Graph tags for social sharing
- ✅ Proper heading hierarchy (H1, H2, H3)
- ✅ Alt text for images (add when you include real images)
- ✅ Fast loading times
- ✅ Mobile-responsive design
- ✅ Clean URLs (when deployed)

### Improving SEO:

1. **Add sitemap.xml:**
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
       <url>
           <loc>https://yourdomain.com/</loc>
           <lastmod>2026-01-24</lastmod>
           <changefreq>monthly</changefreq>
           <priority>1.0</priority>
       </url>
   </urlset>
   ```

2. **Add robots.txt:**
   ```
   User-agent: *
   Allow: /
   Sitemap: https://yourdomain.com/sitemap.xml
   ```

3. **Submit to search engines:**
   - Google Search Console
   - Bing Webmaster Tools

## 🐛 Troubleshooting

### Language not switching:
- Check browser console for errors
- Verify `js/language.js` is loaded
- Clear browser cache and LocalStorage

### Animations not working:
- Check if `css/animations.css` is loaded
- Verify JavaScript is enabled
- Some browsers have "Reduce Motion" enabled in accessibility settings

### Layout issues on mobile:
- Check `css/responsive.css` is loaded
- Use browser DevTools to inspect elements
- Verify viewport meta tag is present

### Navigation not scrolling smoothly:
- Ensure JavaScript is enabled
- Check for console errors
- Verify section IDs match navigation hrefs

## 📝 Content Updates

To update your information:

1. **Personal Information**: Edit in `js/language.js` under `about` section
2. **Work Experience**: Edit `experience.work1`, `work2`, etc.
3. **Education**: Edit `experience.edu1`
4. **Projects**: Edit project cards in `index.html` and translations
5. **Skills**: Edit skills sections in both HTML and translations
6. **Contact Info**: Edit contact section in `index.html`

## 🔄 Version Control

Initialize Git repository:

```bash
git init
git add .
git commit -m "Initial commit: CV portfolio website"
```

Create `.gitignore` file:
```
.DS_Store
Thumbs.db
*.log
node_modules/
.vscode/
.idea/
*.swp
*.swo
```

## 📊 Analytics

### Google Analytics (Optional)

Add before closing `</head>` tag in `index.html`:

```html
<!-- Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```

### Cloudflare Web Analytics (Recommended)

Privacy-friendly, no cookies, GDPR compliant:
- Enable in Cloudflare Pages dashboard
- No code changes needed
- Free on all plans

## 🤝 Contributing

Feel free to fork this project and customize it for your own use!

## 📄 License

This project is open source and available for personal use.

## 📧 Contact

- **Email**: roman.velicka1@gmail.com
- **Phone**: +420 737 657 407
- **LinkedIn**: [linkedin.com/in/romanvelicka](https://linkedin.com/in/romanvelicka/)
- **Location**: Brno, Ostrava region, Czech Republic

## 🎓 About This Project

This portfolio website was created to showcase:
- Network administration and IT support skills
- Web development capabilities using modern technologies
- Self-hosting knowledge (Raspberry Pi deployment)
- Professional experience and certifications

**Built with**: HTML5, CSS3, Vanilla JavaScript
**Font**: Inter (Google Fonts)
**Icons**: Inline SVG
**Hosting**: Self-hosted on Raspberry Pi & Cloudflare Pages

---

**Last Updated**: January 2026
**Version**: 1.0.0

Made with ❤️ by Roman Velička
