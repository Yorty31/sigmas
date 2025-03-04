import requests

def get_wikipedia_info():
    url = "https://en.wikipedia.org/api/rest_v1/page/summary/Brad_Pitt"
    try:
        response = requests.get(url)
        response.raise_for_status()
        data = response.json()
        info = {
            "title": data.get("title", "Невідомо"),
            "description": data.get("description", "Немає опису"),
            "extract": data.get("extract", "Нічого не знайдено"),
            "image": data.get("thumbnail", {}).get("source", "Зображення відсутнє"),
            "page_url": data.get("content_urls", {}).get("desktop", {}).get("page", "URL відсутній"),
            "last_updated": data.get("timestamp", "Дата оновлення відсутня"),
            "wikidata_id": data.get("wikibase_item", "Wikidata ID відсутній")
        }
        return info
    except requests.exceptions.RequestException as e:
        return {"error": f"Помилка: {e}"}

def main():
    info = get_wikipedia_info()
    if "error" in info:
        print(info["error"])
    else:
        print(f"Назва: {info['title']}")
        print(f"Опис: {info['description']}")
        print(f"Витяг: {info['extract']}")
        print(f"Зображення: {info['image']}")
        print(f"Сторінка: {info['page_url']}")
        print(f"Останнє оновлення: {info['last_updated']}")
        print(f"Wikidata ID: {info['wikidata_id']}")

if __name__ == "__main__":
    main()
