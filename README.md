<!-- @format -->

# Rezi ATS Checker SDK

A browser-embeddable SDK that analyzes resumes against ATS (Applicant Tracking System) criteria and provides a scored report with actionable feedback.

## Quick Start

```html
<!DOCTYPE html>
<html>
  <head>
    <!-- Stylesheet is loaded automatically by the SDK -->
  </head>
  <body>
    <!-- 1. Place the container div -->
    <div
      data-ats-checker
      data-dev="false"
      data-cta-url="https://app.rezi.ai/signup"
      data-recaptcha-key="6LeGtEgqAAAAAAi89fMo241vMKH3C9QRLH0a9nzC"
    ></div>

    <!-- 2. Load the SDK script (auto-initializes) -->
    <script src="https://cdn.rezi.ai/sdk/rezi-ats-checker.min.js"></script>
  </body>
</html>
```

### `data-*` Attributes

| Attribute            | Required | Default                        | Description                                    |
| -------------------- | -------- | ------------------------------ | ---------------------------------------------- |
| `data-ats-checker`   | ✅ Yes   | —                              | Triggers SDK auto-initialization               |
| `data-dev`           | ❌ No    | `false`                        | `true` uses dev API + dev reCAPTCHA key        |
| `data-cta-url`       | ❌ No    | `https://app.rezi.ai/signup`   | CTA button link                                |
| `data-recaptcha-key` | ❌ No    | Built-in key                   | Override the reCAPTCHA site key                |

## JavaScript API

For manual initialization with callbacks:

```javascript
const checker = new window.ReziATSChecker.ATSChecker({
  containerSelector: '#my-ats-checker',
  isDev: false,
  ctaUrl: 'https://app.rezi.ai/signup',
  recaptchaSiteKey: '6LeGtEgqAAAAAAi89fMo241vMKH3C9QRLH0a9nzC',

  onFileUpload: (file) => {
    console.log('File selected:', file.name);
  },
  onAnalysisComplete: (result) => {
    console.log('ATS Score:', result.totalScore);
    console.log('Grade:', result.gradeLabel);

    if (result.keywords.mode === 'job_description') {
      console.log('Matched keywords:', result.keywords.matchedKeywords);
      console.log('Missing keywords:', result.keywords.missingKeywords);
    }
  },
  onCTAClick: (result, anonResumeToken) => {
    window.location.href = `/signup?anon_resume_token=${anonResumeToken}`;
  },
  onError: (error) => {
    console.error('ATS check error:', error.message);
  },
});

// Cleanup
// checker.destroy();
```

## Configuration Options

```typescript
interface ATSCheckerConfig {
  containerSelector: string;              // CSS selector (required)
  isDev?: boolean;                        // Dev mode (default: false)
  theme?: 'light' | 'dark' | 'auto';     // Theme (not yet implemented)
  ctaUrl?: string;                        // CTA link URL
  maxFileSize?: number;                   // Max file size in bytes (default: 5MB)
  allowedTypes?: string[];                // Allowed MIME types (default: PDF + DOCX)
  recaptchaSiteKey?: string;              // reCAPTCHA site key override

  onFileUpload?: (file: File) => void;
  onAnalysisComplete?: (result: AnalysisResult) => void;
  onCTAClick?: (result: AnalysisResult, anonResumeToken: string) => void;
  onError?: (error: Error) => void;
}
```

## AnalysisResult Type

```typescript
interface AnalysisResult {
  totalScore: number;                       // 0–100
  grade: 'critical' | 'needs_improvement' | 'ats_ready';
  gradeLabel: string;                       // e.g. "ATS Ready"

  formatting: CategoryResult;               // Format score (maxScore: 50)
  structure: CategoryResult;                // Structure score (maxScore: 30)
  keywords: KeywordResult;                  // Keyword score (maxScore: 20)
  topIssues: CheckResult[];                 // Failed/warning checks
}

interface KeywordResult extends CategoryResult {
  mode: 'job_description' | 'default';
  matchedKeywords?: string[];               // JD mode only
  missingKeywords?: string[];               // JD mode only
  matchPercentage?: number;                 // JD mode only (0–100)
}
```

### Grade Thresholds

| Score Range | grade                | Label                  |
| ----------- | -------------------- | ---------------------- |
| 80–100      | `ats_ready`          | ATS Ready              |
| 50–79       | `needs_improvement`  | Needs Improvement      |
| 0–49        | `critical`           | Critical Issues Found  |

## Build

```bash
# UMD bundle for CDN distribution (run from monorepo root)
nx build-umd ats-checker-sdk

# Output
dist/libs/ats-checker-sdk-browser/rezi-ats-checker.min.js
dist/libs/ats-checker-sdk-browser/rezi-ats-checker.css
```

## Examples

See `example.html` for a complete working example.

## Documentation

- [DEVELOPMENT.md](./DEVELOPMENT.md) — Service architecture, API specs, local setup, and scoring logic
- [ARCHITECTURE.md](./ARCHITECTURE.md) — System design, data flow diagrams, and key design decisions

### Browser Support

- ✅ Chrome 70+
- ✅ Firefox 65+
- ✅ Safari 12+
- ✅ Edge 79+
- ✅ iOS Safari 12+
- ✅ Android Chrome 70+

## 📝 License

MIT License - see LICENSE file for details.

## 🤝 Contributing

- 📧 Email: support@rezi.io

## 📞 Support

- 📧 Email: support@rezi.io

---

Made with ❤️ by the Rezi Team
