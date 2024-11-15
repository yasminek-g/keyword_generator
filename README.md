# Keyword Extraction Process

This project extracts and filters lecture video keywords from the MIT OpenCourseWare (OCW) site. Using a web scraper built with Selenium and `chromedriver`, the script navigates OCW topics to collect course titles and lecture titles associated with each topic. The extraction process involves several steps to ensure relevant, concise, and unique keywords for each course and lecture.

## Steps in Keyword Extraction

### 1. **Web Scraping Setup**
- The script uses Selenium to automate browsing, with `chromedriver` configured to access the OCW search results pages.
- Each topic in a predefined list (e.g., "Engineering," "Mathematics," "Computer Science") is searched for lecture videos on the OCW platform.

---

### 2. **Data Collection and Scrolling**
- For each topic, the scraper constructs a URL to query OCW’s lecture videos and loads the search results page.
- Using infinite scroll, the script simulates user behavior by scrolling down the page, pausing to allow content loading, and triggering lazy loading by scrolling slightly up when needed.

---

### 3. **Extracting Course and Lecture Titles**
- For each loaded page section, the script collects course information by targeting elements containing both the course title (`resource_course_title`) and lecture title (`title`).
- If a video element is present and both fields (`resource_course_title`, `title`) are available, the entry is stored in a unique set to prevent duplicates.

---

### 4. **Keyword Filtering and Processing**
Once data collection is complete, the project applies several filtering processes to clean and refine the collected titles. These include:

#### **4.1. Removing Unnecessary Phrases**
- Common introductory phrases such as "Introduction to" and "Principles of" are stripped from `resource_course_title` fields to retain the core subject.

#### **4.2. Removing Parentheses and Course Codes**
- Any text within parentheses, such as course codes, is removed to focus on the core keywords.

#### **4.3. Removing Punctuation and Special Characters**
- Titles are filtered to remove punctuation-containing words, Roman numerals, and invalid or meaningless strings like "A2."

#### **4.4. Splitting Titles**
- Lecture titles are split by punctuation (e.g., commas, colons, semicolons) and whitespace, ensuring each word or phrase stands as a distinct keyword.

#### **4.5. Deduplication**
- A new function `deduplicate_within_category` ensures no duplicate entries exist within the same category by comparing `resource_course_title` and `title` as unique keys.

#### **4.6. Excluding Continuation Titles**
- Entries with continuation markers such as "(cont.)" or "(continued)" are skipped to avoid partial or incomplete keywords.

---

### 5. **Category Nesting, Cleaning, and Renaming**
New enhancements to the project now include advanced category nesting, cleanup, and renaming functionality:

#### **5.1. Analyzing and Nesting Categories**
- The function `analyze_and_nest_categories` identifies overlaps across categories and nests highly overlapping categories under a parent category.

#### **5.2. Cleaning Parent Categories**
- The function `clean_parent_categories` ensures parent categories do not contain duplicate entries present in their nested subcategories.

#### **5.3. Removing Empty Categories**
- A final cleaning step removes any entries where the `own_lectures` field exists but is empty.

#### **5.4. Renaming `own_lectures`**
- A new function `rename_own_lectures_to_parent` renames `own_lectures` fields to match the parent category name if they are not empty. For example:
  - **Before**: 
    ```json
    {
      "Physics": {
        "own_lectures": [
          {"resource_course_title": "Quantum Mechanics", "title": ["Wave Functions"]}
        ]
      }
    }
    ```
  - **After**:
    ```json
    {
      "Physics": {
        "Physics": [
          {"resource_course_title": "Quantum Mechanics", "title": ["Wave Functions"]}
        ]
      }
    }
    ```

---
### 6. **Reorganizing JSON**
The `reorganize_json` function ensures the data is properly structured by merging and moving subcategories under their respective parent categories. The hierarchy is determined from the initial nesting and through manual verification. 

---
### 7. **Producing a random keyword list for analysis**
The `get_random_unique_words` function picks a number of random titles from the general category under each primary STEM field to produce a keyword list for primliminary analysis. 

---

### 7. **Output**
- After processing, each topic contains a cleaned, de-duplicated, and reorganized list of courses and their lectures. Output files include:
  - **`final_stem_lectures.json`** — Final output with `own_lectures` renamed to parent category names.
  - **`random_unique_words_from_categories.json`** - List of random titles from all general categories of STEM. 

---


## Dependencies

### Python Packages:
- **Selenium**: For web scraping and automation.
- **NLTK**: For stopword removal during filtering.
- **JSON**: (Standard library) For data storage and manipulation.

---
