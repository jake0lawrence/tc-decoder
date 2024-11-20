```markdown
# Master Developer Guide: From RSS Feed to Website and Automated Weekly Newsletter

*As the Chief Technical Officer, this project is designed to transform RSS feeds into a dynamic website and automate a weekly newsletter, using cost-effective tools suitable for a small team with a limited budget.*

## 📋 Table of Contents

1. [Introduction](#introduction)
2. [Project Overview](#project-overview)
3. [Prerequisites](#prerequisites)
4. [Installation](#installation)
5. [Usage](#usage)
    - [Fetching and Parsing RSS Feeds](#fetching-and-parsing-rss-feeds)
    - [Generating Markdown Files](#generating-markdown-files)
    - [Automating Content Updates](#automating-content-updates)
    - [Sending the Weekly Newsletter](#sending-the-weekly-newsletter)
6. [Configuration](#configuration)
7. [Automation and Scheduling](#automation-and-scheduling)
8. [Deployment](#deployment)
9. [Maintenance and Monitoring](#maintenance-and-monitoring)
10. [Advanced Features](#advanced-features)
11. [Contributing](#contributing)
12. [License](#license)
13. [Contact](#contact)
14. [Acknowledgments](#acknowledgments)

---

## Introduction

This project aims to create a system that:

- Aggregates content from an RSS feed.
- Publishes the content on a website using Obsidian Publish.
- Automates a weekly newsletter sent via Sendinblue.

All decisions are made to ensure cost-effectiveness and efficiency, making it ideal for a small team operating on a limited budget.

---

## Project Overview

We will:

- Use Python scripts to fetch and parse RSS feeds.
- Generate markdown files from the parsed content and add them to an Obsidian vault.
- Use Obsidian Publish to host the website.
- Automate the sending of a weekly newsletter using Sendinblue's API.
- Schedule scripts using cron jobs for automation.

---

## Prerequisites

- **Programming Knowledge**: Basic understanding of Python and web development.
- **Tools and Technologies**:
    - Python 3.x
    - Obsidian and Obsidian Publish
    - Sendinblue account
    - Git (optional, for version control)
- **Python Libraries**:
    - `feedparser`
    - `requests`
    - `jinja2`
    - `sib-api-v3-sdk` (Sendinblue API SDK)
- **Accounts**:
    - Obsidian Publish account
    - Sendinblue account with API key

---

## Installation

1. **Clone the Repository**

    ```bash
    git clone https://github.com/yourusername/yourrepository.git
    cd yourrepository
    ```

2. **Set Up a Virtual Environment (Optional but Recommended)**

    ```bash
    python3 -m venv venv
    source venv/bin/activate  # On Windows use `venv\Scripts\activate`
    ```

3. **Install the Required Python Libraries**

    ```bash
    pip install -r requirements.txt
    ```

    *Ensure your `requirements.txt` includes:*

    ```
    feedparser
    requests
    jinja2
    sib-api-v3-sdk
    ```

4. **Set Up Obsidian**

    - Install Obsidian from [https://obsidian.md/](https://obsidian.md/).
    - Create a new vault or use an existing one.

5. **Configure Obsidian Publish**

    - Subscribe to Obsidian Publish.
    - In Obsidian, go to Settings > Obsidian Publish, and set up your site.

6. **Set Up Sendinblue**

    - Sign up at [Sendinblue](https://www.sendinblue.com/).
    - Generate an API key in your account settings.

---

## Usage

### Fetching and Parsing RSS Feeds

The script `fetch_articles.py` fetches articles from a specified RSS feed and parses them.

**Edit `fetch_articles.py` to specify your RSS feed URL:**

```python
feed_url = "https://yourrssfeedurl.com/feed/"
```

**Run the script:**

```bash
python fetch_articles.py
```

### Generating Markdown Files

The script will generate markdown files from the fetched articles and save them in a specified directory within your Obsidian vault.

**Specify the output directory in `fetch_articles.py`:**

```python
output_dir = "/path/to/your/obsidian/vault/Articles"
```

### Automating Content Updates

To automate the fetching and parsing process, schedule `fetch_articles.py` to run periodically.

**Example using cron (runs daily at 6 AM):**

```bash
0 6 * * * /usr/bin/python3 /path/to/fetch_articles.py
```

### Sending the Weekly Newsletter

The script `send_newsletter.py` compiles the top articles from the past week and sends them as a newsletter via Sendinblue.

**Configure Sendinblue API Key and Sender Details in `send_newsletter.py`:**

```python
configuration.api_key['api-key'] = os.getenv('SENDINBLUE_API_KEY')

send_smtp_email = sib_api_v3_sdk.SendSmtpEmail(
    to=[{"email": "recipient@example.com"}],  # Replace with actual recipient or list management
    sender={"name": "Your Name", "email": "your_email@example.com"},
    subject="Weekly Tech News Digest",
    html_content=content
)
```

**Run the script:**

```bash
python send_newsletter.py
```

---

## Configuration

1. **Environment Variables**

    Store sensitive information like API keys in environment variables.

    **Example (.env file):**

    ```
    SENDINBLUE_API_KEY=your_sendinblue_api_key
    ```

    Load them in your scripts using `os.getenv('VARIABLE_NAME')`.

2. **Email Recipients**

    Update the `to` field in `send_newsletter.py` to specify your recipients.

    **Note:** For managing multiple recipients, consider integrating with Sendinblue's contact lists.

3. **Templates**

    Customize the email template in `templates/newsletter.html` to match your branding.

---

## Automation and Scheduling

Use cron jobs to automate the execution of scripts.

- **Fetch Articles Daily at 6 AM**

    ```bash
    0 6 * * * /usr/bin/python3 /path/to/fetch_articles.py
    ```

- **Send Newsletter Every Monday at 9 AM**

    ```bash
    0 9 * * MON /usr/bin/python3 /path/to/send_newsletter.py
    ```

---

## Deployment

- **Website Hosting**

    - Use Obsidian Publish to host your website.
    - After generating new markdown files, open Obsidian and publish the updates.

- **Automate Publishing (Optional)**

    - Currently, Obsidian Publish does not support automatic publishing via scripts.
    - Consider setting reminders to publish updates after scripts run.

---

## Maintenance and Monitoring

- **Logging**

    - Implement logging in your scripts to monitor activity and errors.
    - Example using Python's `logging` module.

- **Error Notifications**

    - Set up email alerts or notifications for script failures.

- **Updating Dependencies**

    - Regularly update your Python libraries to the latest versions.

- **RSS Feed Monitoring**

    - Periodically verify that your RSS feed source is active and has not changed its structure.

---

## Advanced Features

- **Personalization**

    - Collect subscriber preferences to send personalized content.

- **Analytics**

    - Integrate Google Analytics with your Obsidian Publish site.
    - Use Sendinblue's analytics tools to monitor email performance.

- **SEO Optimization**

    - Optimize your website content for search engines by using proper meta tags and headings.

---

## Contributing

Contributions are welcome! Please open an issue or submit a pull request for any improvements or bug fixes.

---

## License

This project is licensed under the MIT License.

---

## Contact

- **Project Maintainer**: [Your Name]
- **Email**: your_email@example.com
- **GitHub**: [https://github.com/yourusername](https://github.com/yourusername)

---

## Acknowledgments

- **Obsidian** for providing a powerful note-taking and publishing platform.
- **Sendinblue** for offering accessible email marketing tools.
- **Feedparser** and **Requests** libraries for simplifying RSS feed handling.

---

*This README.md provides a comprehensive guide to setting up and running the project. For detailed explanations and code samples, refer to the scripts and templates provided in the repository.*
```
