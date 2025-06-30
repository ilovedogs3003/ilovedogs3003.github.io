---
layout: page
title: Deconstructing Monstrosity 
description: Sentiment Analysis for Frankenstein and Dracula 
img: assets/img/frank_tfidf_wc.png
importance: 6
category: Python
related_publications: true
code_url: https://colab.research.google.com/drive/1NLrzWotohLrXkPOwgmgDe3mQR4Wtjr6K?usp=sharing
code_label: Open in Colab
---
<hr>
<p>
    Mary Shelley's <i>Frankenstein; or, The Modern Prometheus</i> is one of my favorite books, I have read it twice the past year alone! The purpose of this project was to gain a better understanding of the contents within by comparing it to one of my least favorite books—Bram Stoker's <i>Dracula</i>—through the use of Natural Language Processing. 
</p>
<h4 style="color:rgb(224, 10, 10);">Data Cleaning & Preprocessing</h4>
<p>
    The analysis begins by importing full-text versions of <i>Frankenstein</i> and <i>Dracula</i> from Project Gutenberg. To ensure meaningful results, non-narrative content (such as prefaces and licensing information) was removed. Each text was then standardized by converting it to lowercase and removing punctuation.
</p>
<p>
    Additional cleaning involved removing common English stopwords (e.g., "the", "and", "was") to isolate the more meaningful vocabulary. The Google Colab notebook provided above includes the entirity of the code and allows you to execute it yourself.
</p>

<h4 style="color:rgb(224, 10, 10);">Summary Table</h4>
<p>
The table below offers a quantitative comparison of <i>Frankenstein</i> and <i>Dracula</i> revealing notable differences in length, vocabulary richness, and overall sentiment.
</p>

<div style="margin-bottom: 4px; width: 100%; text-align: left; margin-bottom: 0;">
    <iframe src="https://ilovedogs3003.github.io/lfs/html/frank_comparison_table.html"
                    width="100%" height="330px" style="border: none; display: block; margin: 0; padding: 0;" loading="eager"></iframe>
</div>
<ul>
  <li><b>Raw Text Length:</b> <i>Dracula</i> is significantly longer, with 163,352 characters compared to <i>Frankenstein</i>'s 72,888. This suggests a denser narrative and likely more complex plot structure.</li>

  <li><b>Total Word Count:</b> After preprocessing, <i>Dracula</i> contains over 71,000 words—more than double that of <i>Frankenstein</i> (33,543). This further confirms the difference in narrative scale.</li>

  <li><b>Unique Word Count:</b> <i>Dracula</i> uses a wider vocabulary, with 9,110 unique words versus 6,579 in <i>Frankenstein</i>. This suggests a greater lexical variety and possibly more dynamic shifts in tone or setting.</li>

  <li><b>Median Word Length:</b> The median word length is slightly higher in <i>Dracula</i> (9) than in <i>Frankenstein</i> (8.5), hinting at slightly more complex word usage.</li>

  <li><b>Lexical Diversity:</b> <i>Dracula</i> exhibits a much higher lexical diversity score (17.64) than <i>Frankenstein</i> (10.87), indicating that it reuses words less often and has more varied language.</li>

  <li><b>Average Sentiment Score:</b> Both novels have slightly negative sentiment scores overall, reflecting their gothic tones. However, <i>Dracula</i> appears marginally more negative (-0.0031) than <i>Frankenstein</i> (-0.0024), possibly due to its themes of fear, predation, and confinement.</li>
</ul>

<h4 style="color:rgb(224, 10, 10);">Analyzing the Text</h4>
<p>
    The distribution of word lengths was plotted for each book, highlighting stylistic differences. For example, <i>Frankenstein</i> used slightly longer, more complex words on average than <i>Dracula</i>.
</p>

<div class="row">
  <div class="col-sm mt-3 mt-md-0 d-flex justify-content-center">
    <div style="max-width: 1000px; width: 100%;">
      {%include figure.liquid 
        loading="eager" 
        path="assets/img/frank_drac_wordlen.png" 
        title="TFA Scatter" 
        class="img-fluid rounded z-depth-0" 
      %}
    </div>
  </div>
</div>
<h5 style="color:rgb(224, 10, 10);">Sentiment Analysis</h5>
<p>
    To better understand the emotional tone of <i>Frankenstein</i> and <i>Dracula</i>, the unique words from each novel were scored using the VADER sentiment analyzer. VADER assigns each word a compound sentiment score ranging from -1 (most negative) to +1 (most positive). In this case, we focused on the <strong>absolute value</strong> of sentiment scores to emphasize emotional intensity regardless of polarity.
</p>
<p>
    The graphs reveal something I suspected (scroll to switch between them):
</p>
<style>
  .scroll-image-container {
    position: relative;
    height: 100px;
  }

  .scroll-image {
    position: absolute;
    width: 100%;
    height: auto;
    transition: opacity 0.5s ease-in-out;
  }

  .scroll-image.hidden {
    opacity: 0;
    pointer-events: none;
  }

  .caption-container {
    position: relative;
    min-height: 100px; /* match image height */
  }

  .scroll-caption {
    position: absolute;
    top: 0;
    left: 0;
    transition: opacity 0.5s ease-in-out;
  }

  .scroll-caption.hidden {
    opacity: 0;
    pointer-events: none;
  }
</style>

<div class="row align-items-start">
  <div class="col-md-8">  <!-- ~66% width for image -->
    <div class="scroll-image-container">
      <img src="/assets/img/frank_drac_sentimentcomp.png" class="scroll-image" id="image-1">
      <img src="/assets/img/frank_drac_sentiment_kde.png" class="scroll-image hidden" id="image-2">
    </div>
  </div>

  <div class="col-md-4">  <!-- ~33% width for caption -->
    <div class="caption-container">
      <!-- Caption 1 -->
      <div class="scroll-caption" id="caption-1">
        <p>
          The first plot displays the <strong>absolute sentiment distribution</strong> of unique words in both novels. Each bar represents the number of emotionally charged words (i.e., with non-zero sentiment) that fall into a particular sentiment intensity range.
        </p>
        <ul>
          <li><span style="color:red;"><strong>Dracula</strong></span> shows a slightly higher frequency of low-to-moderate intensity words (0.1–0.3).</li>
          <li><span style="color:green;"><strong>Frankenstein</strong></span> has a more concentrated spike in the 0.3–0.5 range, indicating a higher use of emotionally intense words in specific sections.</li>
          <li>Both novels cluster most of their emotionally charged language around the middle of the intensity scale, though <i>Frankenstein</i> appears to edge out <i>Dracula</i> slightly in high-intensity usage (above 0.5).</li>
        </ul>
      </div>

      <!-- Caption 2 -->
      <div class="scroll-caption hidden" id="caption-2">
        <p>
          The second plot presents the same data through <strong>kernel density estimation (KDE)</strong>, which smooths out the distribution to show overall patterns. This visualization helps compare the density of sentiment intensity without being influenced by binning decisions. Put simply, it shows us the <strong>relative likelihood</strong> of words from each text appearing in a given sentiment score. 
        </p>
        <ul>
          <li>Both books exhibit a bell-shaped curve peaking around the 0.4 range, reflecting a preference for moderately intense emotional language.</li>
          <li><i>Frankenstein</i> (green) <b>consistently scores slightly higher across the sentiment spectrum</b>, especially between 0.35 and 0.5, reinforcing the idea that Shelley's prose is more emotionally weighted on average.</li>
          <li><i>Dracula</i> (red) demonstrates more fluctuation in the lower range (under 0.3), which may contribute to my dislike of the text (it focuses on a slow-burn rather than intensity).</li>
        </ul>
      </div>
    </div>
  </div>
</div>

<!-- Scroll Trigger -->
<div id="scroll-trigger" style="margin-top: 500px;"></div>

<script>
  const img1 = document.getElementById("image-1");
  const img2 = document.getElementById("image-2");
  const cap1 = document.getElementById("caption-1");
  const cap2 = document.getElementById("caption-2");

  const observer = new IntersectionObserver(entries => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        img1.classList.add("hidden");
        img2.classList.remove("hidden");
        cap1.classList.add("hidden");
        cap2.classList.remove("hidden");
      } else {
        img1.classList.remove("hidden");
        img2.classList.add("hidden");
        cap1.classList.remove("hidden");
        cap2.classList.add("hidden");
      }
    });
  });

  observer.observe(document.getElementById("scroll-trigger"));
</script>
<p>
    Together, these plots reveal that while both novels employ a rich emotional lexicon, <strong><i>Frankenstein</i> leans slightly more toward high-intensity sentiment</strong>, suggesting a more dramatic and perhaps introspective tone. In contrast, <i>Dracula</i>'s emotional expression appears more subdued, which aligns with its suspense-driven narrative style.
</p>
<p>
This pattern is especially evident when we run the analysis on the text as a whole (i.e., no stop-words, but not limiting to unique words).
</p>
<div class="row">
  <div class="col-sm mt-3 mt-md-0 d-flex justify-content-center">
    <div style="max-width: 1000px; width: 100%;">
      {% include figure.liquid 
        loading="eager" 
        path="assets/img/frank_drac_sentiment_kde_allwords.png" 
        title="TFA Scatter" 
        class="img-fluid rounded z-depth-0" 
      %}
    </div>
  </div>
</div>
<p>
    <i>Dracula</i> dominates the lower half of the sentiment intensity scale, while <i>Frankenstein</i> is more prominent in the upper half. This may limit Frankenstein’s lexical diversity, but it also highlights Mary Shelley’s remarkable ability—at just 18 years old—to capture grief, vitriol, and the beauty of humanity with emotional precision.
</p>


<h4 style="color:rgb(224, 10, 10);">Word Clouds</h4>
<p>
    Word clouds were created to highlight the most frequently used content words in each novel. After further refining the stopword lists, themed color palettes were applied—such as a glowing red for <i>Dracula</i> and a cooler twilight scheme for <i>Frankenstein</i>.
</p>
<p>
    Custom-shaped masks (silhouettes of Frankenstein’s monster and Dracula) were used to generate word clouds highlighting the words that were sentiment-intense used most by each author. 
</p>
<div class="mb-3">
  <label for="cloudSelect" class="form-label"><strong>Word Cloud View:</strong></label>
  <select id="cloudSelect" class="form-select" style="max-width: 250px;">
    <option value="sentiment" selected>Sentimentally Charged</option>
    <option value="all">All Words</option>
    <option value="stopwords">Manually Filtered Stop Words</option>
  </select>
</div>

<!-- Sentimentally Charged (DEFAULT) -->
<div class="row" id="cloud-sentiment">
    <div class="col-md-6 text-center">
        <div style="max-width: 500px; margin: 0 auto;">  <!-- Adjust width as needed -->
            <h5>Frankenstein: Sentimentally Charged</h5>
            {% include figure.liquid loading="eager" path="assets/img/frank_absentiment_wc.png" title="Frankenstein Sentiment" class="img-fluid rounded z-depth-0" %}
        </div>
    </div>
    <div class="col-md-6 text-center">
        <div style="max-width: 500px; margin: 0 auto;">
            <h5>Dracula: Sentimentally Charged</h5>
            {% include figure.liquid loading="eager" path="assets/img/drac_absentiment_wc.png" title="Dracula Sentiment" class="img-fluid rounded z-depth-0" %}
        </div>
    </div>
    <div class="col-12 text-center mt-3">
        <p>
            This view filters out emotionally neutral terms, showing only words with non-zero sentiment scores. The resulting clouds emphasize emotionally charged vocabulary across both novels. While <strong>Frankenstein</strong> leans toward sorrow and introspection (<em>murder</em>, <em>suicide</em>, <em>paradise</em>), <strong>Dracula</strong> features sharper contrasts and more polarizing extremes (<em>slavery</em>, <em>hellish</em>, <em>glorious</em>). The images visually convey the tonal intensity embedded within each author's prose.
        </p>
    </div>
</div>


<!-- All Words -->
<div class="row" id="cloud-all" style="display: none;">
    <div class="col-md-6 text-center">
        <h5>Frankenstein: All Words</h5>
        {% include figure.liquid loading="eager" path="assets/img/franktxtclean_wc.png" title="Frankenstein All" class="img-fluid rounded z-depth-0" %}
  </div>
    <div class="col-md-6 text-center">
        <h5>Dracula: All Words</h5>
        {% include figure.liquid loading="eager" path="assets/img/dractxtclean_wc.png" title="Dracula All" class="img-fluid rounded z-depth-0" %}
    </div>
    <div class="col-12 text-center mt-3">
        <p>
            These word clous use <strong>all words</strong> from each novel. While visually engaging, it's dominated by low-intensity, functional terms like <em>"one," "said,"</em> and <em>"may."</em> These frequent but emotionally neutral words make it harder to extract meaningful patterns or tone. Without filtering by sentiment, <strong>emotional nuance is largely flattened</strong>.
        </p>
    </div>
</div>

<!-- Stopword-Filtered -->
<div class="row" id="cloud-stopwords" style="display: none;">
  <div class="col-md-6 text-center">
    <h5>Frankenstein: Manually Selecting Stop Words</h5>
    {% include figure.liquid loading="eager" path="assets/img/franktxtclean_sw_wc.png" title="Frankenstein Stopwords" class="img-fluid rounded z-depth-0" %}
  </div>
  <div class="col-md-6 text-center">
    <h5>Dracula: Manually Selecting Stop Words</h5>
    {% include figure.liquid loading="eager" path="assets/img/dractxtclean_sw_wc.png" title="Dracula Stopwords" class="img-fluid rounded z-depth-0" %}
  </div>
    <div class="col-12 text-center mt-3">
        <p>
            This view removes common yet contextually irrelevant words from each novel, based on a manual review of their top 100 most frequent terms. By filtering more selectively, the resulting clouds reveal greater emotional and thematic clarity. For <strong>Frankenstein</strong>, characters like <em>Elizabeth</em> and themes of <em>misery, death,</em> and <em>hope</em> become more prominent. In contrast, <strong>Dracula</strong> still appears cluttered—names like <em>Jonathan</em> and <em>Van Helsing</em> dominate, but underlying themes remain harder to discern.
        </p>
    </div>
</div>

<script>
  document.getElementById("cloudSelect").addEventListener("change", function () {
    const value = this.value;

    // Hide all
    document.getElementById("cloud-sentiment").style.display = "none";
    document.getElementById("cloud-all").style.display = "none";
    document.getElementById("cloud-stopwords").style.display = "none";

    // Show selected
    if (value === "sentiment") {
      document.getElementById("cloud-sentiment").style.display = "flex";
    } else if (value === "all") {
      document.getElementById("cloud-all").style.display = "flex";
    } else if (value === "stopwords") {
      document.getElementById("cloud-stopwords").style.display = "flex";
    }
  });
</script>

<h4 style="color:rgb(224, 10, 10);">Co-occurrence Heatmaps</h4>
<p>
    Finally, co-occurrence matrices were generated using the top sentiment-bearing words. These matrices visualize which words appear together frequently, uncovering thematic clusters and associations within each text. 
</p>
<div class="row justify-content-sm-center">
  <!-- Image 1 -->
  <div class="col-sm-6 col-md-6 mt-3 mt-md-0">
    <div class="ratio ratio-4x3">
            {% include figure.liquid loading="eager" path="assets/img/frank_cooccurrence_heatmap.png" title="Frankenstein Co-Matrix" class="img-fluid rounded z-depth-0" %}
    </div>
  </div>

  <!-- Image 2 -->
  <div class="col-sm-6 col-md-6 mt-3 mt-md-0">
    <div class="ratio ratio-4x3">
            {% include figure.liquid loading="eager" path="assets/img/drac_cooccurrence_heatmap.png" title="Frankenstein Co-Matrix" class="img-fluid rounded z-depth-0" %}
    </div>
  </div>
</div>
<br>
<br>
<br>
<h4 style="color:rgb(224, 10, 10);">Conclusion</h4>
<p>
    This project not only explores how sentiment and language structure differ between two iconic gothic novels, but also showcases how Natural Language Processing (NLP) techniques can enrich literary analysis. By deconstructing monstrosity through word frequencies, sentiment intensity, and thematic clustering, we gain insight into each author’s emotional landscape and narrative focus.
</p>

