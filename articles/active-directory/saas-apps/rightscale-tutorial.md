---
title: 'Tutorial: Integración de Azure Active Directory con Rightscale | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Rightscale.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 3a8d376d-95fb-4dd7-832a-4fdd4dd7c87c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 301199667d2307bc81da7ef42f3e4f7daa750ee2
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56199712"
---
# <a name="tutorial-azure-active-directory-integration-with-rightscale"></a>Tutorial: Integración de Azure Active Directory con Rightscale

En este tutorial, aprenderá a integrar Rightscale con Azure Active Directory (Azure AD).

Integrar Rightscale con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Rightscale
- Puede permitir que los usuarios inicien sesión automáticamente en Rightscale (Inicio de sesión único) con sus cuentas de Azure AD
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Rightscale, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Rightscale

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Rightscale desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-rightscale-from-the-gallery"></a>Adición de Rightscale desde la galería
Para configurar la integración de Rightscale en Azure AD, deberá agregar Rightscale desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Rightscale desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

1. En el cuadro de búsqueda, escriba **Rightscale**.

    ![Creación de un usuario de prueba de Azure AD](./media/rightscale-tutorial/tutorial_rightscale_search.png)

1. En el panel de resultados, seleccione **Rightscale** y luego haga clic en el botón **Agregar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/rightscale-tutorial/tutorial_rightscale_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD
En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Rightscale con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de RightsScale para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Rightscale.

Para establecer la relación de vínculo, en Rightscale, asigne el valor de **nombre de usuario** de Azure AD como valor de **Nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con Rightscale, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de Rightscale](#creating-a-rightscale-test-user)**: para tener un homólogo de Britta Simon en Rightscale que esté vinculado a su representación en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Rightscale.

**Para configurar el inicio de sesión único de Azure AD con Rightscale, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **Rightscale**, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Configurar inicio de sesión único](./media/rightscale-tutorial/tutorial_rightscale_samlbase.png)

1. En el **Rightscale dominio y las direcciones URL** sección, si desea volver a configurar la aplicación en **modo iniciado por IDP** no es necesario realizar los pasos porque la aplicación ya está integrada previamente con Azure.

    ![Configurar inicio de sesión único](./media/rightscale-tutorial/tutorial_rightscale_url.png)

1. En la sección **Dominio y direcciones URL de Rightscale**, si quiere configurar la aplicación en **SP initiated mode** (Modo iniciado por SP), realice los siguientes pasos:
    
    ![Configurar inicio de sesión único](./media/rightscale-tutorial/tutorial_rightscale_url1.png)

     a. Haga clic en la opción **Mostrar configuración avanzada de URL**.

    b. En el cuadro de texto **Dirección URL de inicio de sesión**, escriba una dirección URL similar a la siguiente: `https://login.rightscale.com/`

1. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Configurar inicio de sesión único](./media/rightscale-tutorial/tutorial_rightscale_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Configurar inicio de sesión único](./media/rightscale-tutorial/tutorial_general_400.png)

1. En la sección **Configuración de Rightscale**, haga clic en **Configurar Rightscale** para abrir la ventana **Configurar inicio de sesión**. Copie los valores de **SAML Entity ID y SAML Single Sign-On Service URL** (Identificador de entidad de SAML y Dirección URL del servicio de inicio de sesión único de SAML) de la sección **Referencia rápida**.

    ![Configuración del inicio de sesión único](./media/rightscale-tutorial/tutorial_rightscale_configure.png) 
<CS>
1. Para configurar SSO para la aplicación, debe iniciar sesión en su inquilino de RightScale como administrador.

     a. En el menú de la parte superior, haga clic en la pestaña **Configuración** y seleccione **Inicio de sesión único**.
   
    ![Configurar inicio de sesión único](./media/rightscale-tutorial/tutorial_rightscale_001.png) 

    b. Haga clic en el botón "**nuevo**" para agregar **sus proveedores de identidades SAML**.
   
    ![Configurar inicio de sesión único](./media/rightscale-tutorial/tutorial_rightscale_002.png) 
 
    c. En el cuadro de texto **Nombre para mostrar**, escriba el nombre de la compañía.
   
    ![Configurar inicio de sesión único](./media/rightscale-tutorial/tutorial_rightscale_003.png)
 
    d. Seleccione **Permitir SSO iniciado por RightScale con una sugerencia de detección** y escriba el **nombre de dominio** en el cuadro de texto siguiente.
   
    ![Configurar inicio de sesión único](./media/rightscale-tutorial/tutorial_rightscale_004.png)

    e. Pegue el valor del **Dirección URL del servicio de inicio de sesión único de SAML** que copió de Azure Portal en **SAML SSO Endpoint** (Punto de conexión SSO de SAML) en Rightscale.
   
    ![Configurar inicio de sesión único](./media/rightscale-tutorial/tutorial_rightscale_006.png)

    f. Pegue el valor del **identificador de entidad de SAML** que copió de Azure Portal en **SAML EntityID** (Identificador de entidad de SAML) en Rightscale.
   
    ![Configurar inicio de sesión único](./media/rightscale-tutorial/tutorial_rightscale_008.png)

    g. Haga clic en el botón **Explorador** para cargar el certificado que descargó de Azure Portal.
   
    ![Configurar inicio de sesión único](./media/rightscale-tutorial/tutorial_rightscale_009.png)

    h. Haga clic en **Save**(Guardar).
<CE>
> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más aquí sobre la característica de documentación insertada: [Documentación insertada de Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/rightscale-tutorial/create_aaduser_01.png) 

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.
    
    ![Creación de un usuario de prueba de Azure AD](./media/rightscale-tutorial/create_aaduser_02.png) 

1. Para abrir el cuadro de diálogo **Usuario**, haga clic en **Agregar** en la parte superior del cuadro de diálogo.
 
    ![Creación de un usuario de prueba de Azure AD](./media/rightscale-tutorial/create_aaduser_03.png) 

1. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Creación de un usuario de prueba de Azure AD](./media/rightscale-tutorial/create_aaduser_04.png) 

     a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="creating-a-rightscale-test-user"></a>Creación de un usuario de prueba de Rightscale

En esta sección, creará un usuario denominado Britta Simon en RightScale. Colabore con el [equipo de soporte técnico de Rightscale](mailto:support@rightscale.com) para agregar usuarios en la plataforma de Rightscale.

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Britta Simon a Rightscale para que use el inicio de sesión único de Azure.

![Asignar usuario][200] 

**Para asignar a Britta Simon a Rightscale, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **Rightscale**.

    ![Configurar inicio de sesión único](./media/rightscale-tutorial/tutorial_rightscale_app.png) 

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202] 

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

El objetivo de esta sección es probar la configuración del inicio de sesión único de Azure AD mediante el panel de acceso.  

Al hacer clic en el icono de RightScale en el Panel de acceso, debería iniciar sesión automáticamente en su aplicación RightScale.

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/rightscale-tutorial/tutorial_general_01.png
[2]: ./media/rightscale-tutorial/tutorial_general_02.png
[3]: ./media/rightscale-tutorial/tutorial_general_03.png
[4]: ./media/rightscale-tutorial/tutorial_general_04.png

[100]: ./media/rightscale-tutorial/tutorial_general_100.png

[200]: ./media/rightscale-tutorial/tutorial_general_200.png
[201]: ./media/rightscale-tutorial/tutorial_general_201.png
[202]: ./media/rightscale-tutorial/tutorial_general_202.png
[203]: ./media/rightscale-tutorial/tutorial_general_203.png

