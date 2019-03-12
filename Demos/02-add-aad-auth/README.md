# <a name="completed-module-add-azure-ad-authentication"></a>Abgeschlossenes Modul: Hinzufügen der Azure AD-Authentifizierung

Die Version des Projekts in diesem Verzeichnis spiegelt das Abschließen des Lernprogramms durch [Hinzufügen der Azure AD-Authentifizierung](https://docs.microsoft.com/graph/training/ruby-tutorial?tutorial-step=3)wider. Wenn Sie diese Version des Projekts verwenden, müssen Sie den Rest des Lernprogramms beginnend beim [Abrufen von Kalenderdaten](https://docs.microsoft.com/graph/training/ruby-tutorial?tutorial-step=4)abschließen.

> **Hinweis:** Es wird davon ausgegangen, dass Sie bereits eine Anwendung im App-Registrierungs Portal registriert haben, wie in [Registrieren der APP im Portal](https://docs.microsoft.com/graph/training/ruby-tutorial?tutorial-step=2)angegeben. Sie müssen diese Version des Beispiels wie folgt konfigurieren:
>
> 1. Benennen Sie `./config/oauth_environment_variables.rb.example` die Datei `oauth_environment_variables.rb`in um.
> 1. Bearbeiten Sie `oauth_environment_variables.rb` die Datei, und nehmen Sie die folgenden Änderungen vor.
>     1. Ersetzen `YOUR_APP_ID_HERE` Sie durch die **Anwendungs-ID** , die Sie im App-Registrierungs Portal erhalten haben.
>     1. Ersetzen `YOUR APP PASSWORD HERE` Sie durch das Kennwort, das Sie im App-Registrierungs Portal erhalten haben.