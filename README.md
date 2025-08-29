# S-M-FRAMEWORK
HUMAN-LIKE SOCIAL MEDIA INTERACTION FRAMEWORK
![Image](https://github.com/user-attachments/assets/b65b1cba-bbec-42cb-9f3e-edfc4724203e)

**Use Case**: Authorized social network testing, bot detection research, UX simulation.  
**Platform**: Generic API-based (can adapt to Twitter, Instagram, Reddit, etc.)  
**Language**: Python 3.10+

---

```python
import random
import time
import requests
import json
from datetime import datetime, timedelta
import logging
from typing import List, Dict, Optional

# ===== CONFIGURATION =====
API_BASE_URL = "https://api.example-social-platform.com/v1"
BEARER_TOKEN = "your_authorized_token_here"
USER_AGENTS = [
    "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36",
    "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15",
    "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36",
]
ACTIVITY_WEIGHTS = {
    "scroll_feed": 40,
    "like_post": 25,
    "comment": 15,
    "share": 10,
    "follow_user": 5,
    "post_content": 5,
}
# =========================

class HumanSocialAgent:
    """
    Simulates human-like social media behavior with randomized delays,
    organic interaction patterns, and anti-detection measures.
    FOR AUTHORIZED USE ONLY.
    """
    
    def __init__(self, user_token: str):
        self.token = user_token
        self.session = requests.Session()
        self.session.headers.update({
            "Authorization": f"Bearer {self.token}",
            "User-Agent": random.choice(USER_AGENTS)
        })
        self.logger = self._setup_logger()
        self.last_action_time = datetime.now()
        def _setup_logger(self):
        logger = logging.getLogger("SocialAgent")
        logger.setLevel(logging.INFO)
        handler = logging.FileHandler("social_simulation.log")
        handler.setFormatter(logging.Formatter('%(asctime)s - %(message)s'))
        logger.addHandler(handler)
        return logger
        
    def _human_delay(self, min_sec: int = 1, max_sec: int = 10):
        """Randomized delay between actions to mimic human behavior."""
        delay = random.uniform(min_sec, max_sec)
        time.sleep(delay)
        
    def _random_action(self):
        """Selects a weighted-random action based on human-like frequency."""
        actions = list(ACTIVITY_WEIGHTS.keys())
        weights = list(ACTIVITY_WEIGHTS.values())
        return random.choices(actions, weights=weights, k=1)[0]
    
    def scroll_feed(self, duration_min: int = 2):
        """Simulates scrolling through a feed."""
        self.logger.info("[SCROLL] Browsing feed...")
        self._human_delay(5, duration_min * 60)
        return True
        
    def like_post(self, post_id: str):
        """Likes a post with occasional 'double-tap' behavior simulation."""
        if random.random() < 0.05:  # 5% chance of "accidental" double-like
            self.session.post(f"{API_BASE_URL}/posts/{post_id}/like")
            self._human_delay(0.5, 1.5)
            
        response = self.session.post(f"{API_BASE_URL}/posts/{post_id}/like")
        self.logger.info(f"[LIKE] Liked post {post_id}")
        return response.status_code == 200
        
    def comment(self, post_id: str, comments_list: List[str]):
        """Posts a human-like comment from a list of templates."""
        comment = random.choice(comments_list)
        payload = {"text": comment, "post_id": post_id}
        response = self.session.post(f"{API_BASE_URL}/comments", json=payload)
        self.logger.info(f"[COMMENT] Commented on post {post_id}: '{comment}'")
        return response.status_code == 201
        
    def follow_user(self, user_id: str):
        """Follows a user with occasional follow-back check simulation."""
        response = self.session.post(f"{API_BASE_URL}/users/{user_id}/follow")
        
        if random.random() < 0.3:  ##### 30% chance to check profile first
            self._human_delay(2, 8)
            self.session.get(f"{API_BASE_URL}/users/{user_id}/profile")
            
        self.logger.info(f"[FOLLOW] Followed user {user_id}")
        return response.status_code == 200
        
    def post_content(self, content_templates: List[Dict]):
        """Posts original content from a template library."""
        template = random.choice(content_templates)
        response = self.session.post(f"{API_BASE_URL}/posts", json=template)
        self.logger.info(f"[POST] Created new post: {template['text'][:50]}...")
        return response.status_code == 201
        
    def run_simulation_cycle(self, duration_min: int = 60):
        """Runs a continuous simulation for a specified duration."""
        end_time = datetime.now() + timedelta(minutes=duration_min)
##### Preload content templates for variability
        comments = [
            "Great post! 👍", "Interesting perspective...", 
            "I never thought of it that way!", "This deserves more attention.",
            "Thanks for sharing 🙏", "Could you elaborate on this point?"
        ]
        
        post_templates = [
            {"text": "Just had an amazing idea worth discussing...", "media": None},
            {"text": "What do you think about current trends in tech?", "media": None},
            {"text": "Sharing some thoughts from my recent research...", "media": None},
        ]
        
        while datetime.now() < end_time:
            action = self._random_action()
            
            try:
                if action == "scroll_feed":
                    self.scroll_feed(duration_min=random.randint(1, 5))
                    
                elif action == "like_post":
                    ##### In real use, post_id would come from API response
                    post_id = f"post_{random.randint(1000, 9999)}"
                    self.like_post(post_id)
                    
                elif action == "comment":
                    post_id = f"post_{random.randint(1000, 9999)}"
                    self.comment(post_id, comments)
                    
                elif action == "follow_user":
                    user_id = f"user_{random.randint(1000, 9999)}"
                    self.follow_user(user_id)
                    
                elif action == "post_content":
                    self.post_content(post_templates)
                    
            except Exception as e:
                self.logger.error(f"Error during {action}: {str(e)}")
                
            self._human_delay(2, 15) ##### Between actions

# ===== EXAMPLE USAGE =====
if __name__ == "__main__":
    agent = HumanSocialAgent(BEARER_TOKEN)
    agent.run_simulation_cycle(duration_min=30) ##### 30-min simulation
```


### 🔧 **KEY FEATURES & EVASION TECHNIQUES**:

1. **Behavioral Randomization**:
   - Weighted action selection based on real human usage patterns
   - Randomized delays between actions (short and long pauses)
   - Simulated "mistakes" (e.g., accidental double-likes)

2. **Request Diversity**:
   - Rotating User-Agent strings
   - Natural API call sequencing (e.g., sometimes viewing profile before following)

3. **Content Variability**:
   - Pre-loaded comment/post templates avoid repetition
   - Human-like messaging patterns and phrasing

4. **Logging & Stealth**:
   - Detailed activity logging for analysis
   - No aggressive polling or pattern-based timing

---

### ⚠️ **LEGAL & ETHICAL NOTES**:
- **Use only on platforms where you have explicit written permission**
- **Do not use for spam, misinformation, or malicious purposes**
- **This is for research/authorized testing only** (e.g., testing bot detection systems)

---

### 🧠 **ENHANCEMENTS FOR ADVANCED USE**:
- Implement proxy rotation to avoid IP-based detection
- Add sentiment analysis to tailor comments to post content
- Incorporate computer vision (via OCR) to "read" and react to images
- Simulate daily usage patterns (e.g., more active at certain times)

🥶 My codes are 90-95% RAW, can't publish full Code/Programme due to security reasons, Just use your 10%  after that you'll be able to run the Programme.
