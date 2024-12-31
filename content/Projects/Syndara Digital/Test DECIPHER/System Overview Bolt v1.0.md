## System Overview

### File Processing

- Accepts .zip files containing project content
- Requires `config.json` with specific structure:
    
    ```
    {
      package_name: string
      version: string
      author: string
      created_at: string
      sections: Section[]
    }
    ```
    
- Files are processed based on type (md, pdf, json, csv, images)
- Content is stored in localStorage for persistence

### Section Structure

Five required sections, ordered:

1. Home
2. Agents
3. Insights
4. Resources
5. Actions

### File Type Handling

- Documents (.md, .pdf) - Rendered with Markdown/PDF viewers
- Charts (.json) - Visualized with Recharts
- Tables (.csv) - Displayed in structured grid
- Images - Rendered with Next.js Image component
- URLs - Displayed as clickable links

## UI/UX Design

### Layout

- Dark theme with purple accents
- Gradient background (purple-900 to black)
- Fixed navigation sidebar (width: 80px)
- Content area with responsive padding

### Navigation

- Vertical icon menu for sections
- Icons reveal labels on hover
- Active section highlighted
- Mobile-responsive collapsible menu

### Content Display

- Card-based layout with consistent styling
- Backdrop blur effects
- Proper spacing between elements
- Loading states with progress indicators

### Error Handling

- Toast notifications for errors
- Graceful fallbacks for unsupported files
- Clear error messages with actions
- Loading screens for processing

### Interactions

1. File Upload:
    
    - Drag & drop or click to select
    - Progress indicator during processing
    - Success/error feedback via toast
2. Navigation:
    
    - Smooth transitions between sections
    - Persistent state across navigation
    - Back/forward browser navigation support
3. Content Viewing:
    
    - Interactive charts
    - Downloadable PDFs
    - Responsive image scaling
    - Table sorting/filtering

### Responsive Design

- Mobile-first approach
- Flexible grid layouts
- Collapsible navigation on small screens
- Touch-friendly interactions

This overview reflects the current implementation while highlighting the core functionality and design principles of DECIPHER.