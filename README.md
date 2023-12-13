# NLP-Dream-Analysis-Chatbot
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.naive_bayes import MultinomialNB
from sklearn.pipeline import make_pipeline

def train_text_classification_model(data_path):
    # Load the dataset from CSV
    df = pd.read_csv(data_path)

    # Assume your CSV has two columns: "Dream" and "Meaning"
    X = df["Dream"]
    y = df["Meaning"]

    # Split the data into training and testing sets
    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

    # Create a simple text classification model
    model = make_pipeline(CountVectorizer(), MultinomialNB())
    model.fit(X_train, y_train)

    return model

def get_user_input():
    return input("Spill the cosmic beans about your dream, or type 'exit' to escape this dreamy conversation: ")

def classify_dream(model, user_dream):
    # Unraveling the dreamy riddles...
    vectorizer = model.named_steps['countvectorizer']
    user_dream_representation = vectorizer.transform([user_dream])
    nonzero_indices = user_dream_representation.nonzero()[1]
    keywords = [vectorizer.get_feature_names_out()[index] for index in nonzero_indices]

    # Deciphering the quirky codes...
    keyword_meanings = {keyword: model.predict([keyword])[0] for keyword in keywords}

    # Crafting a quirky summary of the dream universe...
    all_meanings = set(keyword_meanings.values())
    summary = ", ".join(all_meanings)

    return keywords, summary

def main():
    print("Greetings, dream voyager! Ready to embark on a quirky quest into the whimsical world of dreams?")

    data_path = "Dreamzzzzz.csv"
    model = train_text_classification_model(data_path)

    while True:
        user_dream = get_user_input()

        if user_dream.lower() == 'exit':
            print("Dream on, cosmic explorer! Until our quirky paths cross again.")
            break

        keywords, summary = classify_dream(model, user_dream)

        # Unveiling the quirky enigmas...
        print("\nHold onto your cosmic hat! Let me decode the quirkiness of your dream...")

        if not keywords:
            print("Oopsie-daisy! Couldn't catch any specific themes in your description. How about sprinkling more quirk?")
        else:
            print("\nVoila! I sensed some quirky elements in your dream: {}".format(", ".join(keywords)))
            print("\nIn a nutshell, your dream might be related to: {}".format(summary))

        print("\n" + "=" * 50)  # Adding a separator for an extra dash of quirk

if __name__ == "__main__":
    main()
