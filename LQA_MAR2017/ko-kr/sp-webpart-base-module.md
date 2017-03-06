# <a name="sp-webpart-base-module"></a>sp-webpart-base 모듈



## <a name="classes"></a>클래스

| 클래스    |  설명 |
|:-------------|:---------------|
| [`BaseClientSideWebPart`](./sp-webpart-base/baseclientsidewebpart.md)     | 이 추상 클래스는 클라이언트 쪽 웹 파트의 기본 기능을 구현합니다. 모든 클라이언트 쪽 웹 파트는 이 클래스에서 상속해야 합니다. 기본 기능과 함께, 이 클래스는 웹 파트에 사용할 수 있는 몇 가지 API를 제공합니다. 이러한 API는 두 가지 범주에 속합니다. 첫 번째 API 범주는 데이터와 기능을 제공합니다. 예로 웹 파트 컨텍스트(예: this.context)가 있습니다. 이 API는 이 웹 파트 인스턴스와 관련된 상황에 맞는 데이터에 액세스하는 데 사용됩니다. 두 번째 API 범주는 웹 파트 수명 주기에 대한 기본 구현을 제공하며 업데이트된 구현에 따라 재정의될 수 있습니다. render() API는 웹 파트에 의해 구현/재정의되어야 하는 유일한 API입니다. 다른 모든 수명 주기 API는 기본 구현을 가지며, 웹 파트의 필요에 따라 재정의될 수 있습니다. 올바른 결정을 내리려면 개별 API 설명서를 참조하세요. |



## <a name="interfaces"></a>인터페이스

| 인터페이스    |  설명 |
|:-------------|:---------------|
| [`IClientSideWebPartStatusRenderer`](./sp-webpart-base/iclientsidewebpartstatusrenderer.md)   | 웹 파트에 대해 로드 표시기와 오류 메시지를 표시해야 하는 구성 요소에 의해 구현될 인터페이스입니다.  |
| [`IPlaceholderProps`](./sp-webpart-base/iplaceholderprops.md)   | 콘텐츠가 없거나 임시 콘텐츠만 있는 경우 자리 표시자를 표시하는 데 사용됩니다. 단추는 선택적 요소입니다.  |
| [`IPlaceholderSpinnerProps`](./sp-webpart-base/iplaceholderspinnerprops.md)   | 웹 파트 표시 영역에 로드 회전자를 표시하는 데 사용되는 속성에 대한 인터페이스입니다.  |
| [`IPropertyPaneButtonProps`](./sp-webpart-base/ipropertypanebuttonprops.md)   | PropertyPane 단추 속성  |
| [`IPropertyPaneCheckboxProps`](./sp-webpart-base/ipropertypanecheckboxprops.md)   | PropertyPane CheckBox 구성 요소 속성  |
| [`IPropertyPaneChoiceGroupOption`](./sp-webpart-base/ipropertypanechoicegroupoption.md)   | PropertyPane ChoiceGroup 옵션 속성  |
| [`IPropertyPaneChoiceGroupProps`](./sp-webpart-base/ipropertypanechoicegroupprops.md)   | PropertyPane ChoiceGroup 속성  |
| [`IPropertyPaneConfiguration`](./sp-webpart-base/ipropertypaneconfiguration.md)   | 웹 파트 구성 설정  |
| [`IPropertyPaneCustomFieldProps`](./sp-webpart-base/ipropertypanecustomfieldprops.md)   | PropertyPane CustomPropertyField 속성  |
| [`IPropertyPaneDropdownOption`](./sp-webpart-base/ipropertypanedropdownoption.md)   | PropertyPane 드롭다운 옵션  |
| [`IPropertyPaneDropdownProps`](./sp-webpart-base/ipropertypanedropdownprops.md)   | PropertyPane 드롭다운 구성 요소 속성  |
| [`IPropertyPaneField`](./sp-webpart-base/ipropertypanefield.md)   | PropertyPane 필드  |
| [`IPropertyPaneGroup`](./sp-webpart-base/ipropertypanegroup.md)   | PropertyPane 그룹 Group은 PropertyPanePage의 일부입니다.  |
| [`IPropertyPaneLabelProps`](./sp-webpart-base/ipropertypanelabelprops.md)   | PropertyPaneLabel 구성 요소 속성  |
| [`IPropertyPaneLinkProps`](./sp-webpart-base/ipropertypanelinkprops.md)   | PropertyPaneLink 구성 요소 속성  |
| [`IPropertyPanePage`](./sp-webpart-base/ipropertypanepage.md)   | PropertyPanePage 인터페이스  |
| [`IPropertyPanePageHeader`](./sp-webpart-base/ipropertypanepageheader.md)   | PropertyPane 헤더 이 헤더는 모든 페이지에 동일한 상태로 유지됩니다.  |
| [`IPropertyPaneSliderProps`](./sp-webpart-base/ipropertypanesliderprops.md)   | PropertyPaneSliderProps 구성 요소 속성  |
| [`IPropertyPaneTextFieldProps`](./sp-webpart-base/ipropertypanetextfieldprops.md)   | PropertyPaneTextField 구성 요소 속성  |
| [`IPropertyPaneToggleProps`](./sp-webpart-base/ipropertypanetoggleprops.md)   | PropertyPaneToggle 구성 요소 속성  |
| [`ISerializedServerProcessedData`](./sp-webpart-base/iserializedserverprocesseddata.md)   | 검색 인덱스 및 링크 수정 같은 서버 쪽 서비스에서 처리할 수 있는 데이터의 컬렉션을 포함합니다.  |
| [`ISerializedWebPartData`](./sp-webpart-base/iserializedwebpartdata.md)   | 이 구조는 웹 파트에 의해 제어되는 웹 직렬화 상태 부분을 나타냅니다. 프레임워크에서 직렬화된 데이터에 추가한 추가 데이터를 포함하는 IWebPartData에 의해 확장됩니다.  |
| [`IWebPartContext`](./sp-webpart-base/iwebpartcontext.md)   | 클라이언트 쪽 웹 파트에 대한 기본 컨텍스트 인터페이스입니다.  |
| [`IWebPartData`](./sp-webpart-base/iwebpartdata.md)   | 이 구조는 웹 파트의 직렬화 상태를 나타냅니다. 웹 파트에서 serialize() API가 호출되면 이 구조가 출력됩니다. 'properties' 필드의 구조는 웹 파트에 의해 소유되며 해당 웹 파트에 국한됩니다. 각 웹 파트는 직렬화하려는 속성 집합을 결정할 수 있습니다.  |
| [`IWebPartPropertiesMetadata`](./sp-webpart-base/iwebpartpropertiesmetadata.md)   | 이 구조는 웹 파트 속성에 대한 메타데이터를 IWebPartPropertyMetadata에 대한 문자열 맵으로 정의하는 데 사용됩니다. 키는 웹 파트 속성에서 속성에 대한 json 경로여야 합니다. 이 json 경로는 다음과 같은 연산자를 지원합니다. 점(.) - 개체 멤버를 선택하는 데 사용합니다(예: person.name). 괄호([]) - 배열 항목을 선택하는 데 사용합니다(예: person.photoURLs[0]). 대괄호로 묶은 별표([*]) - 배열 요소 와일드카드에 사용합니다(예: person.websites[*]). 이러한 연산자를 조합해서 사용할 수 있습니다(예: person.websites[*].url). 중요 참고 사항: 경로당 1개의 와일드카드만 지원됩니다. 예제: 다음과 같은 스키마를 갖는 속성의 웹 파트가 있다고 가정해보겠습니다. { title: string; person: { name: string; bio: string; photoURLs: string[]; websites: { title: string; url: string; }[] } } 원하는 속성의 메타데이터를 다음과 같이 정의할 수 있습니다. { 'person.bio': { isRichContent: true }, 'person.photoURLs[*]': { isImageSource: true }, 'person.websites[*].url': { isLink: true } } 이렇게 하면 SharePoint 서버는 사용자 속성의 콘텐츠를 인식할 수 있게 되고 데이터에 대해 검색 인덱싱, 링크 수정 등의 서비스를 실행할 수 있게 됩니다. 링크 수정과 같은 서비스에서 값을 업데이트해야 할 경우 웹 파트 속성 모음이 자동으로 업데이트됩니다.  |



## <a name="functions"></a>함수

| 함수     | 반환값 | 설명 |
|:-------------|:------|:---------------|
| [`PropertyPaneButton(targetProperty,properties)`](./sp-webpart-base/propertypanebutton.md) |[`IPropertyPaneField`](./sp-webpart-base/ipropertypanefield.md)<[`IPropertyPaneButtonProps`](./sp-webpart-base/ipropertypanebuttonprops.md)>  | PropertyPane에 단추를 만들기 위한 도우미 메서드  |
| [`PropertyPaneCheckbox(targetProperty,properties)`](./sp-webpart-base/propertypanecheckbox.md) |[`IPropertyPaneField`](./sp-webpart-base/ipropertypanefield.md)<[`IPropertyPaneCheckboxProps`](./sp-webpart-base/ipropertypanecheckboxprops.md)>  | PropertyPane에 확인란을 만들기 위한 도우미 메서드  |
| [`PropertyPaneChoiceGroup(targetProperty,properties)`](./sp-webpart-base/propertypanechoicegroup.md) |[`IPropertyPaneField`](./sp-webpart-base/ipropertypanefield.md)<[`IPropertyPaneChoiceGroupProps`](./sp-webpart-base/ipropertypanechoicegroupprops.md)>  | PropertyPane에 선택 그룹을 만들기 위한 도우미 메서드  |
| [`PropertyPaneDropdown(targetProperty,properties)`](./sp-webpart-base/propertypanedropdown.md) |[`IPropertyPaneField`](./sp-webpart-base/ipropertypanefield.md)<[`IPropertyPaneDropdownProps`](./sp-webpart-base/ipropertypanedropdownprops.md)>  | PropertyPane에 드롭다운을 만들기 위한 도우미 메서드  |
| [`PropertyPaneHorizontalRule(properties)`](./sp-webpart-base/propertypanehorizontalrule.md) |[`IPropertyPaneField`](./sp-webpart-base/ipropertypanefield.md)<void>  | PropertyPane에 가로줄을 만들기 위한 도우미 메서드  |
| [`PropertyPaneLabel(targetProperty,properties)`](./sp-webpart-base/propertypanelabel.md) |[`IPropertyPaneField`](./sp-webpart-base/ipropertypanefield.md)<[`IPropertyPaneLabelProps`](./sp-webpart-base/ipropertypanelabelprops.md)>  | PropertyPane에 레이블을 만들기 위한 도우미 메서드  |
| [`PropertyPaneLink(targetProperty,properties)`](./sp-webpart-base/propertypanelink.md) |[`IPropertyPaneField`](./sp-webpart-base/ipropertypanefield.md)<[`IPropertyPaneLinkProps`](./sp-webpart-base/ipropertypanelinkprops.md)>  | PropertyPane에 링크를 만들기 위한 도우미 메서드  |
| [`PropertyPaneSlider(targetProperty,properties)`](./sp-webpart-base/propertypaneslider.md) |[`IPropertyPaneField`](./sp-webpart-base/ipropertypanefield.md)<[`IPropertyPaneSliderProps`](./sp-webpart-base/ipropertypanesliderprops.md)>  | PropertyPane에 슬라이더를 만들기 위한 도우미 메서드  |
| [`PropertyPaneTextField(targetProperty,properties)`](./sp-webpart-base/propertypanetextfield.md) |[`IPropertyPaneField`](./sp-webpart-base/ipropertypanefield.md)<[`IPropertyPaneTextFieldProps`](./sp-webpart-base/ipropertypanetextfieldprops.md)>  | PropertyPane에 텍스트 필드를 만들기 위한 도우미 메서드  |
| [`PropertyPaneToggle(targetProperty,properties)`](./sp-webpart-base/propertypanetoggle.md) |[`IPropertyPaneField`](./sp-webpart-base/ipropertypanefield.md)<[`IPropertyPaneToggleProps`](./sp-webpart-base/ipropertypanetoggleprops.md)>  | PropertyPane에 토글을 만들기 위한 도우미 메서드  |


## <a name="enumerations"></a>열거형

| 열거형      | 설명|
|:-----------|:------------|
|[`PropertyPaneButtonType`](./sp-webpart-base/propertypanebuttontype.md)    | 지원되는 모든 단추 형식의 열거형입니다. |
|[`PropertyPaneFieldType`](./sp-webpart-base/propertypanefieldtype.md)    | 지원되는 모든 PropertyPane 필드 형식의 열거형입니다. 이름은 office-ui-fabric-react의 이름과 일치해야 하므로 대/소문자를 올바르게 사용해야 합니다. |




