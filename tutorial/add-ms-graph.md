<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="00c16-101">In dieser Übung integrieren Sie Microsoft Graph in die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="00c16-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="00c16-102">Für diese Anwendung verwenden Sie den [httparty](https://github.com/jnunemaker/httparty) -Edelstein, um Aufrufe von Microsoft Graph zu tätigen.</span><span class="sxs-lookup"><span data-stu-id="00c16-102">For this application, you will use the [httparty](https://github.com/jnunemaker/httparty) gem to make calls to Microsoft Graph.</span></span>

## <a name="create-a-graph-helper"></a><span data-ttu-id="00c16-103">Erstellen einer Graph-Hilfe</span><span class="sxs-lookup"><span data-stu-id="00c16-103">Create a Graph helper</span></span>

<span data-ttu-id="00c16-104">Erstellen Sie eine Hilfsfunktion, um alle Ihre API-Aufrufe zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="00c16-104">Create a helper to manage all of your API calls.</span></span> <span data-ttu-id="00c16-105">Führen Sie den folgenden Befehl in der CLI aus, um den Helfer zu generieren.</span><span class="sxs-lookup"><span data-stu-id="00c16-105">Run the following command in your CLI to generate the helper.</span></span>

```Shell
rails generate helper Graph
```

<span data-ttu-id="00c16-106">Öffnen Sie die neu `./app/helpers/graph_helper.rb` erstellte Datei, und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="00c16-106">Open the newly created `./app/helpers/graph_helper.rb` file and replace the contents with the following.</span></span>

```ruby
require 'httparty'

# Graph API helper methods
module GraphHelper
  GRAPH_HOST = 'https://graph.microsoft.com'.freeze

  def make_api_call(endpoint, token, params = nil)
    headers = {
      Authorization: "Bearer #{token}"
    }

    query = params || {}

    HTTParty.get "#{GRAPH_HOST}#{endpoint}",
                 headers: headers,
                 query: query
  end
end
```

<span data-ttu-id="00c16-107">Nehmen Sie sich einen Moment Zeit, um zu sehen, was dieser Code macht.</span><span class="sxs-lookup"><span data-stu-id="00c16-107">Take a moment to review what this code does.</span></span> <span data-ttu-id="00c16-108">Sie stellt eine einfache GET-Anforderung über `httparty` das gem an den angeforderten Endpunkt.</span><span class="sxs-lookup"><span data-stu-id="00c16-108">It makes a simple GET request via the `httparty` gem to the requested endpoint.</span></span> <span data-ttu-id="00c16-109">Sie sendet das Zugriffstoken in der `Authorization` Kopfzeile und enthält alle übergebenen Abfrageparameter.</span><span class="sxs-lookup"><span data-stu-id="00c16-109">It sends the access token in the `Authorization` header, and it includes any query parameters that are passed.</span></span>

<span data-ttu-id="00c16-110">Wenn Sie beispielsweise die `make_api_call` Methode zum Abrufen von `https://graph.microsoft.com/v1.0/me?$select=displayName`verwenden möchten, können Sie Sie wie folgt aufrufen:</span><span class="sxs-lookup"><span data-stu-id="00c16-110">For example, to use the `make_api_call` method to do a GET to `https://graph.microsoft.com/v1.0/me?$select=displayName`, you could call it like so:</span></span>

```ruby
make_api_call `/v1.0/me`, access_token, { '$select': 'displayName' }
```

<span data-ttu-id="00c16-111">Sie werden später darauf aufbauen, wenn Sie mehr Microsoft Graph-Features in die APP implementieren.</span><span class="sxs-lookup"><span data-stu-id="00c16-111">You'll build on this later as you implement more Microsoft Graph features into the app.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="00c16-112">Abrufen von Kalenderereignissen aus Outlook</span><span class="sxs-lookup"><span data-stu-id="00c16-112">Get calendar events from Outlook</span></span>

<span data-ttu-id="00c16-113">Beginnen wir mit dem Hinzufügen der Möglichkeit, Ereignisse im Kalender des Benutzers anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="00c16-113">Let's start by adding the ability to view events on the user's calendar.</span></span> <span data-ttu-id="00c16-114">Führen Sie in der CLI den folgenden Befehl aus, um einen neuen Controller hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="00c16-114">In your CLI, run the following command to add a new controller.</span></span>

```Shell
rails generate controller Calendar index
```

<span data-ttu-id="00c16-115">Nachdem die Route verfügbar ist, aktualisieren Sie den Link **Kalender** in der navbar, `./app/view/layouts/application.html.erb` um Sie zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="00c16-115">Now that we have the route available, update the **Calendar** link in the navbar in `./app/view/layouts/application.html.erb` to use it.</span></span> <span data-ttu-id="00c16-116">Ersetzen Sie die `<a class="nav-link" href="#">Calendar</a>` -Reihe durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="00c16-116">Replace the line `<a class="nav-link" href="#">Calendar</a>` with the following.</span></span>

```html
<%= link_to "Calendar", {:controller => :calendar, :action => :index}, class: "nav-link#{' active' if controller.controller_name == 'calendar'}" %>
```

<span data-ttu-id="00c16-117">Fügen Sie der Graph-Hilfe eine neue Methode hinzu, um [die Ereignisse des Benutzers](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/api/user_list_events)aufzulisten.</span><span class="sxs-lookup"><span data-stu-id="00c16-117">Add a new method to the Graph helper to [list the user's events](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/api/user_list_events).</span></span> <span data-ttu-id="00c16-118">Öffnen `./app/helpers/graph_helper.rb` Sie das `GraphHelper` Modul, und fügen Sie die folgende Methode hinzu.</span><span class="sxs-lookup"><span data-stu-id="00c16-118">Open `./app/helpers/graph_helper.rb` and add the following method to the `GraphHelper` module.</span></span>

```ruby
def get_calendar_events(token)
  get_events_url = '/v1.0/me/events'

  query = {
    '$select': 'subject,organizer,start,end',
    '$orderby': 'createdDateTime DESC'
  }

  response = make_api_call get_events_url, token, query

  raise response.parsed_response.to_s || "Request returned #{response.code}" unless response.code == 200
  response.parsed_response['value']
end
```

<span data-ttu-id="00c16-119">Überlegen Sie sich, was dieser Code tut.</span><span class="sxs-lookup"><span data-stu-id="00c16-119">Consider what this code is doing.</span></span>

- <span data-ttu-id="00c16-120">Die URL, die aufgerufen wird, `/v1.0/me/events`lautet.</span><span class="sxs-lookup"><span data-stu-id="00c16-120">The URL that will be called is `/v1.0/me/events`.</span></span>
- <span data-ttu-id="00c16-121">Der `$select` Parameter schränkt die für jedes Ereignis zurückgegebenen Felder auf diejenigen ein, die unsere Ansicht tatsächlich verwendet.</span><span class="sxs-lookup"><span data-stu-id="00c16-121">The `$select` parameter limits the fields returned for each events to just those our view will actually use.</span></span>
- <span data-ttu-id="00c16-122">Der `$orderby` Parameter sortiert die Ergebnisse nach dem Datum und der Uhrzeit, zu denen Sie erstellt wurden, wobei das neueste Element zuerst angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="00c16-122">The `$orderby` parameter sorts the results by the date and time they were created, with the most recent item being first.</span></span>
- <span data-ttu-id="00c16-123">Für eine erfolgreiche Antwort gibt es das Array von Elementen zurück, die `value` im Schlüssel enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="00c16-123">For a successful response, it returns the array of items contained in the `value` key.</span></span>

<span data-ttu-id="00c16-124">Jetzt können Sie dies testen.</span><span class="sxs-lookup"><span data-stu-id="00c16-124">Now you can test this.</span></span> <span data-ttu-id="00c16-125">Öffnen `./app/controllers/calendar_controller.rb` und aktualisieren Sie `index` die Aktion, um diese Methode aufzurufen und die Ergebnisse zu rendern.</span><span class="sxs-lookup"><span data-stu-id="00c16-125">Open `./app/controllers/calendar_controller.rb` and update the `index` action to call this method and render the results.</span></span>

```ruby
# Calendar controller
class CalendarController < ApplicationController
  include GraphHelper

  def index
    @events = get_calendar_events access_token || []
    render json: @events
  rescue RuntimeError => e
    @errors = [
      {
        message: 'Microsoft Graph returned an error getting events.',
        debug: e
      }
    ]
  end
end
```

<span data-ttu-id="00c16-126">Starten Sie den Server neu.</span><span class="sxs-lookup"><span data-stu-id="00c16-126">Restart the server.</span></span> <span data-ttu-id="00c16-127">Melden Sie sich an, und klicken Sie in der Navigationsleiste auf den Link **Kalender** .</span><span class="sxs-lookup"><span data-stu-id="00c16-127">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="00c16-128">Wenn alles funktioniert, sollte ein JSON-Dump von Ereignissen im Kalender des Benutzers angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="00c16-128">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="00c16-129">Anzeigen der Ergebnisse</span><span class="sxs-lookup"><span data-stu-id="00c16-129">Display the results</span></span>

<span data-ttu-id="00c16-130">Jetzt können Sie HTML und CSS hinzufügen, um die Ergebnisse benutzerfreundlicher anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="00c16-130">Now you can add HTML and CSS to display the results in a more user-friendly manner.</span></span>

<span data-ttu-id="00c16-131">Öffnen `./app/views/calendar/index.html.erb` Sie den Inhalt, und ersetzen Sie ihn durch den folgenden.</span><span class="sxs-lookup"><span data-stu-id="00c16-131">Open `./app/views/calendar/index.html.erb` and replace its contents with the following.</span></span>

```html
<h1>Calendar</h1>
<table class="table">
  <thead>
    <tr>
      <th scope="col">Organizer</th>
      <th scope="col">Subject</th>
      <th scope="col">Start</th>
      <th scope="col">End</th>
    </tr>
  </thead>
  <tbody>
    <% @events.each do |event| %>
      <tr>
        <td><%= event['organizer']['emailAddress']['name'] %></td>
        <td><%= event['subject'] %></td>
        <td><%= event['start']['dateTime'].to_time(:utc).localtime.strftime('%-m/%-d/%y %l:%M %p') %></td>
        <td><%= event['end']['dateTime'].to_time(:utc).localtime.strftime('%-m/%-d/%y %l:%M %p') %></td>
      </tr>
    <% end %>
  </tbody>
</table>
```

<span data-ttu-id="00c16-132">, Der eine Auflistung von Ereignissen durchläuft und jeweils eine Tabellenzeile hinzufügt.</span><span class="sxs-lookup"><span data-stu-id="00c16-132">That will loop through a collection of events and add a table row for each one.</span></span> <span data-ttu-id="00c16-133">Entfernen Sie `render json: @events` die Leitung aus `index` der Aktion `./app/controllers/calendar_controller.rb` in, und die APP sollte jetzt eine Tabelle mit Ereignissen rendern.</span><span class="sxs-lookup"><span data-stu-id="00c16-133">Remove the `render json: @events` line from the `index` action in `./app/controllers/calendar_controller.rb` and the app should now render a table of events.</span></span>

![Screenshot der Ereignistabelle](./images/add-msgraph-01.png)