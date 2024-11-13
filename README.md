# Keyword Extraction Process

This project extracts and filters lecture video keywords from the MIT OpenCourseWare (OCW) site. Using a web scraper built with Selenium and `chromedriver`, the script navigates OCW topics to collect course titles and lecture titles associated with each topic. The extraction process involves several steps to ensure relevant, concise, and unique keywords for each course and lecture.

## Steps in Keyword Extraction

1. **Web Scraping Setup**: 
    - The script uses Selenium to automate browsing, with `chromedriver` configured to access the OCW search results pages.
    - Each topic in a predefined list (e.g., "Engineering," "Mathematics," "Computer Science") is searched for lecture videos on the OCW platform.

2. **Data Collection and Scrolling**:
    - For each topic, the scraper constructs a URL to query OCW’s lecture videos and loads the search results page.
    - Using infinite scroll, the script simulates user behavior by scrolling down the page, pausing to allow content loading, and triggering lazy loading by scrolling slightly up when needed.

3. **Extracting Course and Lecture Titles**:
    - For each loaded page section, the script collects course information by targeting elements containing both the course title (`resource_course_title`) and lecture title (`title`).
    - If a video element is present and both fields (`resource_course_title`, `title`) are available, the entry is stored in a unique set to prevent duplicates.

4. **Keyword Filtering**:
    - Once data collection is complete, the project applies several filtering processes to clean up and refine the collected titles. This includes:
        - **Removing Unnecessary Phrases**: Common introductory phrases such as "Introduction to" and "Principles of" are stripped from `resource_course_title` fields to retain the core subject.
        - **Removing Parentheses and Course Codes**: Any text within parentheses, such as course codes, is removed to focus on the core keywords.
        - **Removing Punctuation and Special Characters**: Titles are filtered to remove punctuation-containing words and Roman numerals for consistency.
        - **Splitting Titles**: Lecture titles are split by punctuation (e.g., commas, colons, semicolons) and whitespace, ensuring each word or phrase stands as a distinct keyword. This breakdown improves keyword readability and relevance.
        - **Stopword Removal**: The script removes common stopwords and lecture-specific terms (e.g., "lecture," "seminar," "overview") to keep only meaningful terms.
    - **Excluding Continuation Titles**: Entries with continuation markers such as "(cont.)" or "(continued)" are skipped to avoid partial or incomplete keywords.

5. **Output**:
    - After processing, each topic contains a cleaned, de-duplicated list of courses and their lectures, formatted in a JSON file (`lecture_videos_data.json`). This file provides an organized, searchable record of keywords by topic.

## Usage

- **Run Scraper**: The scraper initiates by defining topics in a list, running Selenium with headless Chrome to speed up data collection in a production environment.
- **Process and Filter Keywords**: Once data collection is complete, the filtering script processes `resource_course_title` and `title` fields to retain only the most informative keywords.
- **Output JSON**: The final JSON output, `lecture_videos_data.json`, is saved with structured, clean data ready for further analysis or display.

## Dependencies

- **Python Packages**:
  - Selenium
  - NLTK (for stopword filtering)
  - JSON (standard library for data storage)
