# Dakota Digital Design - WordPress Integration Guide

This guide will help you integrate the converted HTML/CSS/JavaScript website into WordPress. The website has been fully converted from React/TypeScript to vanilla HTML, CSS, and JavaScript for easy WordPress integration.

## 🎯 What's Been Converted

- ✅ **React Components** → **HTML Sections**
- ✅ **TypeScript** → **Vanilla JavaScript**
- ✅ **Tailwind CSS** → **Custom CSS with CSS Variables**
- ✅ **React Router** → **Smooth Scrolling Navigation**
- ✅ **Form Handling** → **JavaScript Form Validation & Submission**
- ✅ **Responsive Design** → **Mobile-First CSS**
- ✅ **Animations** → **CSS Animations & JavaScript Interactions**

## 📁 Files Structure

```
dakota-digital-design-webpage-main/
├── index.html          # Main HTML file (can be converted to WordPress template)
├── styles.css          # Complete CSS styling
├── script.js           # JavaScript functionality
├── public/             # Assets folder
│   ├── favicon.ico
│   ├── lovable-uploads/
│   │   └── cb8e6f4d-163d-4940-9973-3d90d13ccf5c.png
│   └── src/assets/
│       ├── hero-image.jpg
│       └── portfolio-mockup.jpg
└── README-WORDPRESS.md # This file
```

## 🚀 WordPress Integration Methods

### Method 1: Custom WordPress Theme (Recommended)

#### Step 1: Create Theme Directory
1. Navigate to `wp-content/themes/` in your WordPress installation
2. Create a new folder: `dakota-digital-design`
3. Copy all files into this folder

#### Step 2: Create Required WordPress Files

**style.css** (Theme Header)
```css
/*
Theme Name: Dakota Digital Design
Description: Professional website design for North Dakota businesses
Version: 1.0
Author: Your Name
*/

/* Import your main styles */
@import url('styles.css');
```

**index.php** (Main Template)
```php
<?php get_header(); ?>

<main>
    <!-- Hero Section -->
    <section id="home" class="hero-section">
        <?php get_template_part('template-parts/hero'); ?>
    </section>

    <!-- Services Section -->
    <section id="services" class="services-section">
        <?php get_template_part('template-parts/services'); ?>
    </section>

    <!-- Portfolio Section -->
    <section id="portfolio" class="portfolio-section">
        <?php get_template_part('template-parts/portfolio'); ?>
    </section>

    <!-- Contact Section -->
    <section id="contact" class="contact-section">
        <?php get_template_part('template-parts/contact'); ?>
    </section>
</main>

<?php get_footer(); ?>
```

**header.php**
```php
<!DOCTYPE html>
<html <?php language_attributes(); ?>>
<head>
    <meta charset="<?php bloginfo('charset'); ?>">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title><?php wp_title('|', true, 'right'); ?></title>
    <?php wp_head(); ?>
</head>
<body <?php body_class(); ?>>

<!-- Navigation -->
<nav class="navigation fixed top-0 left-0 right-0 z-50">
    <div class="container">
        <div class="nav-content">
            <!-- Logo -->
            <div class="logo">
                <img src="<?php echo get_template_directory_uri(); ?>/public/lovable-uploads/cb8e6f4d-163d-4940-9973-3d90d13ccf5c.png" alt="Dakota Digital Design" class="logo-img">
                <span class="logo-text">Dakota Digital Design</span>
            </div>

            <!-- Navigation Tabs -->
            <div class="nav-tabs hidden-md">
                <div class="tabs-list">
                    <button class="tab-trigger active" data-section="home">Home</button>
                    <button class="tab-trigger" data-section="services">Services</button>
                    <button class="tab-trigger" data-section="portfolio">Portfolio</button>
                    <button class="tab-trigger" data-section="contact">Contact</button>
                </div>
            </div>

            <!-- Mobile CTA -->
            <div class="mobile-cta">
                <button class="contact-btn" onclick="scrollToSection('contact')">Contact</button>
            </div>
        </div>
    </div>
</nav>
```

**footer.php**
```php
<!-- Footer content from your HTML -->
<footer class="footer">
    <!-- Copy footer HTML here -->
</footer>

<?php wp_footer(); ?>
</body>
</html>
```

**functions.php**
```php
<?php
// Enqueue scripts and styles
function dakota_digital_enqueue_scripts() {
    wp_enqueue_style('dakota-digital-style', get_template_directory_uri() . '/styles.css');
    wp_enqueue_script('dakota-digital-script', get_template_directory_uri() . '/script.js', array(), '1.0', true);
    
    // Add Google Fonts
    wp_enqueue_style('google-fonts', 'https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap');
}
add_action('wp_enqueue_scripts', 'dakota_digital_enqueue_scripts');

// Add theme support
function dakota_digital_theme_support() {
    add_theme_support('title-tag');
    add_theme_support('post-thumbnails');
    add_theme_support('custom-logo');
    add_theme_support('html5', array('search-form', 'comment-form', 'comment-list', 'gallery', 'caption'));
}
add_action('after_setup_theme', 'dakota_digital_theme_support');

// Contact form handling
function handle_contact_form() {
    if ($_POST['action'] === 'submit_contact_form') {
        // Process form data
        $name = sanitize_text_field($_POST['name']);
        $email = sanitize_email($_POST['email']);
        $phone = sanitize_text_field($_POST['phone']);
        $business = sanitize_text_field($_POST['business']);
        $website = esc_url_raw($_POST['website']);
        $project = sanitize_textarea_field($_POST['project']);
        $timeline = sanitize_text_field($_POST['timeline']);
        
        // Send email or save to database
        $to = get_option('admin_email');
        $subject = 'New Website Request from ' . $name;
        $message = "Name: $name\nEmail: $email\nPhone: $phone\nBusiness: $business\nWebsite: $website\nProject: $project\nTimeline: $timeline";
        
        wp_mail($to, $subject, $message);
        
        wp_die('Success! We\'ll be in touch within 2 hours.');
    }
}
add_action('wp_ajax_submit_contact_form', 'handle_contact_form');
add_action('wp_ajax_nopriv_submit_contact_form', 'handle_contact_form');
?>
```

#### Step 3: Create Template Parts
Create a `template-parts` folder and split your HTML into reusable parts:

**template-parts/hero.php**
```php
<div class="hero-background">
    <div class="hero-overlay"></div>
</div>

<div class="container">
    <div class="hero-grid">
        <!-- Copy hero section HTML here -->
    </div>
</div>
```

**template-parts/services.php**
```php
<div class="container">
    <!-- Copy services section HTML here -->
</div>
```

**template-parts/portfolio.php**
```php
<div class="container">
    <!-- Copy portfolio section HTML here -->
</div>
```

**template-parts/contact.php**
```php
<div class="container">
    <!-- Copy contact section HTML here -->
</div>
```

### Method 2: Page Builder Integration

#### Elementor
1. Create a new page
2. Use Elementor's HTML widget to paste your HTML sections
3. Add custom CSS in Elementor's custom CSS section
4. Use Elementor's form widget for the contact form

#### WPBakery Page Builder
1. Create a new page
2. Use the "Raw HTML" element to paste your HTML
3. Add custom CSS in the page's custom CSS field

### Method 3: WordPress Page Template

1. Create a custom page template
2. Copy your HTML into the template
3. Replace static content with WordPress functions
4. Use WordPress hooks for dynamic content

## 🔧 Customization Options

### Making Content Editable

#### 1. Hero Section
```php
<h1 class="hero-title">
    <?php echo get_theme_mod('hero_title', 'Get More Local Customers with a Professional Website'); ?>
</h1>
<p class="hero-description">
    <?php echo get_theme_mod('hero_description', 'Dakota Digital Design builds websites that convert visitors into customers for North Dakota businesses.'); ?>
</p>
```

#### 2. Services Section
```php
<?php
$services = get_theme_mod('services_list', array(
    array(
        'icon' => '🌐',
        'title' => 'Professional Website Design',
        'description' => 'Custom websites that represent your North Dakota business professionally...',
        'features' => array('Mobile-Responsive', 'Fast Loading', 'Professional Design')
    )
    // Add more services
));

foreach ($services as $service) : ?>
    <div class="service-card">
        <div class="service-icon"><?php echo $service['icon']; ?></div>
        <h3 class="service-title"><?php echo $service['title']; ?></h3>
        <p class="service-description"><?php echo $service['description']; ?></p>
        <!-- Features loop -->
    </div>
<?php endforeach; ?>
```

#### 3. Portfolio Section
```php
<?php
$portfolio_items = get_posts(array(
    'post_type' => 'portfolio',
    'posts_per_page' => 3
));

foreach ($portfolio_items as $item) : ?>
    <div class="portfolio-card">
        <div class="portfolio-image">
            <?php if (has_post_thumbnail($item->ID)) : ?>
                <?php echo get_the_post_thumbnail($item->ID, 'medium'); ?>
            <?php endif; ?>
        </div>
        <div class="portfolio-content">
            <h3 class="portfolio-title"><?php echo $item->post_title; ?></h3>
            <p class="portfolio-description"><?php echo $item->post_excerpt; ?></p>
        </div>
    </div>
<?php endforeach; ?>
```

### Customizer Integration

Add to `functions.php`:
```php
function dakota_digital_customize_register($wp_customize) {
    // Hero Section
    $wp_customize->add_section('hero_section', array(
        'title' => 'Hero Section',
        'priority' => 30,
    ));
    
    $wp_customize->add_setting('hero_title', array(
        'default' => 'Get More Local Customers with a Professional Website',
        'sanitize_callback' => 'sanitize_text_field',
    ));
    
    $wp_customize->add_control('hero_title', array(
        'label' => 'Hero Title',
        'section' => 'hero_section',
        'type' => 'text',
    ));
    
    // Add more customizer options...
}
add_action('customize_register', 'dakota_digital_customize_register');
```

## 📱 Mobile Optimization

The CSS is already mobile-optimized with:
- Responsive breakpoints
- Mobile-first design
- Touch-friendly interactions
- Optimized typography for small screens

## 🎨 Styling Customization

### CSS Variables
All colors and design tokens use CSS variables for easy customization:

```css
:root {
    --primary: #3b82f6;        /* Dakota Blue */
    --success: #10b981;        /* Success Green */
    --accent: #f59e0b;         /* Accent Yellow */
    --background: #ffffff;      /* Background */
    --foreground: #374151;     /* Text Color */
}
```

### Customizing Colors
1. Modify CSS variables in `styles.css`
2. Use WordPress customizer to change colors
3. Add custom CSS classes for specific elements

## 📧 Contact Form Integration

### Option 1: WordPress AJAX
The JavaScript already includes AJAX form handling. Update the form action:

```html
<form class="contact-form" id="contactForm" action="<?php echo admin_url('admin-ajax.php'); ?>">
    <input type="hidden" name="action" value="submit_contact_form">
    <!-- Form fields -->
</form>
```

### Option 2: Contact Form 7
Replace the form with Contact Form 7 shortcode:
```php
<?php echo do_shortcode('[contact-form-7 id="123" title="Contact Form"]'); ?>
```

### Option 3: Gravity Forms
Use Gravity Forms and style with CSS classes.

## 🚀 Performance Optimization

### 1. Image Optimization
- Compress images using tools like TinyPNG
- Use WebP format with fallbacks
- Implement lazy loading

### 2. CSS/JS Optimization
- Minify CSS and JavaScript
- Use WordPress's built-in minification
- Implement critical CSS

### 3. Caching
- Install caching plugins (WP Rocket, W3 Total Cache)
- Enable browser caching
- Use CDN for assets

## 🔍 SEO Optimization

### 1. Meta Tags
```php
<meta name="description" content="<?php echo get_theme_mod('meta_description', 'Professional websites for North Dakota businesses'); ?>">
<meta name="keywords" content="<?php echo get_theme_mod('meta_keywords', 'website design, North Dakota, business websites'); ?>">
```

### 2. Schema Markup
Add structured data for local business:
```php
<script type="application/ld+json">
{
    "@context": "https://schema.org",
    "@type": "LocalBusiness",
    "name": "Dakota Digital Design",
    "address": {
        "@type": "PostalAddress",
        "addressLocality": "Valley City",
        "addressRegion": "ND"
    },
    "telephone": "(701) 840-9830"
}
</script>
```

### 3. Local SEO
- Google My Business integration
- Local keyword optimization
- Location-based content

## 🧪 Testing Checklist

- [ ] Test on multiple devices and browsers
- [ ] Verify form submission works
- [ ] Check mobile navigation
- [ ] Test smooth scrolling
- [ ] Validate HTML/CSS
- [ ] Test performance (PageSpeed Insights)
- [ ] Check accessibility (WCAG compliance)
- [ ] Test contact form validation
- [ ] Verify responsive design
- [ ] Test loading states

## 🆘 Troubleshooting

### Common Issues

1. **Styles not loading**
   - Check file paths in `functions.php`
   - Verify CSS file permissions
   - Clear WordPress cache

2. **JavaScript errors**
   - Check browser console for errors
   - Verify jQuery dependencies
   - Test in different browsers

3. **Images not displaying**
   - Check image file paths
   - Verify file permissions
   - Use WordPress media library

4. **Form not working**
   - Check AJAX setup
   - Verify form action URL
   - Test form validation

### Support Resources

- WordPress Codex: https://codex.wordpress.org/
- WordPress Developer Handbook: https://developer.wordpress.org/
- CSS-Tricks: https://css-tricks.com/
- MDN Web Docs: https://developer.mozilla.org/

## 📈 Next Steps

1. **Content Management**: Make content editable through WordPress admin
2. **Dynamic Portfolio**: Create custom post type for portfolio items
3. **Blog Integration**: Add blog functionality if needed
4. **Analytics**: Integrate Google Analytics and tracking
5. **Backup**: Set up regular backups and security measures
6. **Maintenance**: Plan for regular updates and maintenance

## 🎉 Success!

Your Dakota Digital Design website is now fully converted and ready for WordPress integration! The website maintains all the original functionality while being completely editable through WordPress.

For additional customization or support, refer to the WordPress documentation or contact your developer.
