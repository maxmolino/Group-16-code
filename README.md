# Group-16-code
classdef aneurysm_app_2023 < matlab.apps.AppBase

    % Properties that correspond to app components
    properties (Access = public)
        UIFigure                        matlab.ui.Figure
        AgeEditField                    matlab.ui.control.EditField
        AgeEditFieldLabel               matlab.ui.control.Label
        FamilyHistoryofIADropDown       matlab.ui.control.DropDown
        FamilyHistoryofIADropDownLabel  matlab.ui.control.Label
        HypertensionDropDown            matlab.ui.control.DropDown
        HypertensionDropDownLabel       matlab.ui.control.Label
        RaceDropDown                    matlab.ui.control.DropDown
        RaceDropDownLabel               matlab.ui.control.Label
        SexDropDown                     matlab.ui.control.DropDown
        SexDropDownLabel                matlab.ui.control.Label
        PatientIDEditField              matlab.ui.control.EditField
        PatientIDEditFieldLabel         matlab.ui.control.Label
        SmokerDropDown                  matlab.ui.control.DropDown
        SmokerDropDownLabel             matlab.ui.control.Label
        NormalizeButton                 matlab.ui.control.Button
        LoadDataButton                  matlab.ui.control.Button
    end

    
    properties (Access = private)
        G_data % Global gene data 
    end
    

    % Callbacks that handle component events
    methods (Access = private)

        % Code that executes after component creation
        function startupFcn(app)
            
        end

        % Button pushed function: LoadDataButton
        function LoadDataButtonPushed(app, event)
            [file,path] = uigetfile({'*.csv'});
            full_path = strcat(path,'/',file);
            gene_data = readtable(full_path,'ReadRowNames',true,'ReadVariableNames',true);
            disp(gene_data);
            app.G_data = [gene_data];
            %imshow(a_data,'Parent',app.UIAxes_A);  
        end

        % Button pushed function: NormalizeButton
        function NormalizeButtonPushed(app, event)
            [file,path] = uigetfile({'*.csv'});
            full_path = strcat(path, '/', file);
            gene_data = readtable(full_path, 'Format','auto');
            [file,path] = uigetfile({'*.csv'});
            full_path = strcat(path, '/', file);
            gene_data1 = readtable(full_path, 'Format','auto');
            
            T = join(gene_data,gene_data1);
            disp(T);

            T.Disease = categorical(T.Disease);
            rowidx = T.Disease == 'Aneurysm';
            result = T(rowidx, :);
            disp(result);
            
            fig = app.UIFigure;
            fig.Position = [119,500,800,500];
            ax = uiaxes(fig);
            
            G = groupcounts(result, 'Age', [0 20 30 40 50 60 70 80 90]);
            disp(G.disc_Age);
            disp(G.GroupCount);

            y_line_1=G.GroupCount;
            x_line_1=G.disc_Age;
            plot(ax,x_line_1,y_line_1);
            
            xlabel(ax,'Age');
            ylabel(ax,'Count of Aneurysm by Age');
            title(ax, 'Age vs Risk of Aneurysm');

        end

        % Value changed function: SmokerDropDown
        function SmokerDropDownValueChanged(app, event)
           [file,path] = uigetfile({'*.csv'});
            full_path = strcat(path, '/', file);
            gene_data = readtable(full_path, 'Format','auto','ReadRowNames',true, 'ReadVariableNames',true);
            value = app.SmokerDropDown.Value;
            disp(value);
            gene_data.Smoking = categorical(gene_data.Smoking);
            rowidx = gene_data.Smoking == value;
            result=gene_data(rowidx, :);
            fig = app.UIFigure;
            fig.Position = [119,500,800,500];
            uit = uitable(fig, 'Data', result);
            app.G_data = [gene_data];
        end

        % Value changed function: PatientIDEditField
        function PatientIDEditFieldValueChanged(app, event)
            value = app.PatientIDEditField.Value;
            [file,path] = uigetfile({'*.csv'});
            full_path = strcat(path, '/', file);
            gene_data = readtable(full_path, 'ReadRowNames',true, 'ReadVariableNames',true);
            disp(gene_data(value,:));
            fig = app.UIFigure;
            fig.Position = [119,500,800,500];
            uit = uitable(fig, 'Data', gene_data(value,:));
            app.G_data = gene_data;
        end

        % Value changed function: HypertensionDropDown
        function HypertensionDropDownValueChanged(app, event)
            [file,path] = uigetfile({'*.csv'});
            full_path = strcat(path, '/', file);
            gene_data = readtable(full_path, 'Format','auto','ReadRowNames',true, 'ReadVariableNames',true);
            value = app.HypertensionDropDown.Value;
            disp(value);
            gene_data.Hypertension_1_Yes_ = categorical(gene_data.Hypertension_1_Yes_);
            rowidx = gene_data.Hypertension_1_Yes_ == value;
            result=gene_data(rowidx, :);
            fig = app.UIFigure;
            fig.Position = [119,500,800,500];
            uit = uitable(fig, 'Data', result);
            app.G_data = [gene_data];
        end

        % Value changed function: FamilyHistoryofIADropDown
        function FamilyHistoryofIADropDownValueChanged(app, event)
           [file,path] = uigetfile({'*.csv'});
            full_path = strcat(path, '/', file);
            gene_data = readtable(full_path, 'Format','auto','ReadRowNames',true, 'ReadVariableNames',true);
            value = app.FamilyHistoryofIADropDown.Value;
            disp(value);
            gene_data.FamilyHistoryOfIA_1_Yes_ = categorical(gene_data.FamilyHistoryOfIA_1_Yes_);
            rowidx = gene_data.FamilyHistoryOfIA_1_Yes_ == value;
            result=gene_data(rowidx, :);
            fig = app.UIFigure;
            fig.Position = [119,500,800,500];
            uit = uitable(fig, 'Data', result);
            app.G_data = [gene_data];
        end

        % Callback function
        function FamilyHistoryofIADropDownOpening(app, event)
            
        end

        % Value changed function: SexDropDown
        function SexDropDownValueChanged(app, event)
            [file,path] = uigetfile({'*.csv'});
            full_path = strcat(path, '/', file);
            gene_data = readtable(full_path, 'Format','auto','ReadRowNames',true, 'ReadVariableNames',true);
            value = app.SexDropDown.Value;
            disp(value);
            gene_data.Sex_1_F_ = categorical(gene_data.Sex_1_F_);
            rowidx = gene_data.Sex_1_F_ == value;
            result=gene_data(rowidx, :);
            fig = app.UIFigure;
            fig.Position = [119,500,800,500];
            uit = uitable(fig, 'Data', result);
            app.G_data = [gene_data];
        end

        % Value changed function: RaceDropDown
        function RaceDropDownValueChanged(app, event)
            [file,path] = uigetfile({'*.csv'});
            full_path = strcat(path, '/', file);
            gene_data = readtable(full_path, 'Format','auto','ReadRowNames',true, 'ReadVariableNames',true);
            value = app.RaceDropDown.Value;
            disp(value);
            gene_data.Race = categorical(gene_data.Race);
            rowidx = gene_data.Race == value;
            result=gene_data(rowidx, :);
            fig = app.UIFigure;
            fig.Position = [119,500,800,500];
            uit = uitable(fig, 'Data', result);
            pause(2)
            set(uit,'ColumnWidth',{50})
            app.G_data = [gene_data];
        end

        % Value changed function: AgeEditField
        function AgeEditFieldValueChanged(app, event)
            [file,path] = uigetfile({'*.csv'});
            full_path = strcat(path, '/', file);
            gene_data = readtable(full_path, 'Format','auto','ReadRowNames',true, 'ReadVariableNames',true);
            value = app.AgeEditField.Value;
            disp(value);
            gene_data.Age = categorical(gene_data.Age);
            rowidx = gene_data.Age == value;
            result=gene_data(rowidx, :);
            fig = app.UIFigure;
            fig.Position = [119,500,800,500];
            uit = uitable(fig, 'Data', result);
            app.G_data = [gene_data];
        end
    end

    % Component initialization
    methods (Access = private)

        % Create UIFigure and components
        function createComponents(app)

            % Create UIFigure and hide until all components are created
            app.UIFigure = uifigure('Visible', 'off');
            app.UIFigure.Position = [100 100 800 600];
            app.UIFigure.Name = 'UI Figure';

            % Create LoadDataButton
            app.LoadDataButton = uibutton(app.UIFigure, 'push');
            app.LoadDataButton.ButtonPushedFcn = createCallbackFcn(app, @LoadDataButtonPushed, true);
            app.LoadDataButton.FontWeight = 'bold';
            app.LoadDataButton.Position = [22 559 100 22];
            app.LoadDataButton.Text = 'Load Data';

            % Create NormalizeButton
            app.NormalizeButton = uibutton(app.UIFigure, 'push');
            app.NormalizeButton.ButtonPushedFcn = createCallbackFcn(app, @NormalizeButtonPushed, true);
            app.NormalizeButton.FontWeight = 'bold';
            app.NormalizeButton.Position = [135 558 102 23];
            app.NormalizeButton.Text = 'Normalize';

            % Create SmokerDropDownLabel
            app.SmokerDropDownLabel = uilabel(app.UIFigure);
            app.SmokerDropDownLabel.HorizontalAlignment = 'right';
            app.SmokerDropDownLabel.FontWeight = 'bold';
            app.SmokerDropDownLabel.Position = [212 511 113 22];
            app.SmokerDropDownLabel.Text = 'Smoker';

            % Create SmokerDropDown
            app.SmokerDropDown = uidropdown(app.UIFigure);
            app.SmokerDropDown.Items = {'Yes', 'No', 'Former'};
            app.SmokerDropDown.ValueChangedFcn = createCallbackFcn(app, @SmokerDropDownValueChanged, true);
            app.SmokerDropDown.FontWeight = 'bold';
            app.SmokerDropDown.Position = [340 511 100 22];
            app.SmokerDropDown.Value = 'Yes';

            % Create PatientIDEditFieldLabel
            app.PatientIDEditFieldLabel = uilabel(app.UIFigure);
            app.PatientIDEditFieldLabel.HorizontalAlignment = 'right';
            app.PatientIDEditFieldLabel.FontWeight = 'bold';
            app.PatientIDEditFieldLabel.Position = [262 558 61 22];
            app.PatientIDEditFieldLabel.Text = 'Patient ID';

            % Create PatientIDEditField
            app.PatientIDEditField = uieditfield(app.UIFigure, 'text');
            app.PatientIDEditField.ValueChangedFcn = createCallbackFcn(app, @PatientIDEditFieldValueChanged, true);
            app.PatientIDEditField.Position = [338 557 103 24];

            % Create SexDropDownLabel
            app.SexDropDownLabel = uilabel(app.UIFigure);
            app.SexDropDownLabel.HorizontalAlignment = 'right';
            app.SexDropDownLabel.FontWeight = 'bold';
            app.SexDropDownLabel.Position = [-20 511 113 22];
            app.SexDropDownLabel.Text = 'Sex';

            % Create SexDropDown
            app.SexDropDown = uidropdown(app.UIFigure);
            app.SexDropDown.Items = {'Male', 'Female', 'NR'};
            app.SexDropDown.ItemsData = {'0', '1', 'NR'};
            app.SexDropDown.ValueChangedFcn = createCallbackFcn(app, @SexDropDownValueChanged, true);
            app.SexDropDown.FontWeight = 'bold';
            app.SexDropDown.Position = [108 511 100 22];
            app.SexDropDown.Value = '0';

            % Create RaceDropDownLabel
            app.RaceDropDownLabel = uilabel(app.UIFigure);
            app.RaceDropDownLabel.HorizontalAlignment = 'right';
            app.RaceDropDownLabel.FontWeight = 'bold';
            app.RaceDropDownLabel.Position = [468 512 34 22];
            app.RaceDropDownLabel.Text = 'Race';

            % Create RaceDropDown
            app.RaceDropDown = uidropdown(app.UIFigure);
            app.RaceDropDown.Items = {'White', 'African American', 'NR', 'American Indian/ Alaskan Native, White'};
            app.RaceDropDown.ValueChangedFcn = createCallbackFcn(app, @RaceDropDownValueChanged, true);
            app.RaceDropDown.FontWeight = 'bold';
            app.RaceDropDown.Position = [517 502 81 41];
            app.RaceDropDown.Value = 'White';

            % Create HypertensionDropDownLabel
            app.HypertensionDropDownLabel = uilabel(app.UIFigure);
            app.HypertensionDropDownLabel.HorizontalAlignment = 'right';
            app.HypertensionDropDownLabel.FontWeight = 'bold';
            app.HypertensionDropDownLabel.Position = [22 446 82 22];
            app.HypertensionDropDownLabel.Text = 'Hypertension';

            % Create HypertensionDropDown
            app.HypertensionDropDown = uidropdown(app.UIFigure);
            app.HypertensionDropDown.Items = {'Yes', 'No', 'NR'};
            app.HypertensionDropDown.ItemsData = {'1', '0', 'NR'};
            app.HypertensionDropDown.ValueChangedFcn = createCallbackFcn(app, @HypertensionDropDownValueChanged, true);
            app.HypertensionDropDown.FontWeight = 'bold';
            app.HypertensionDropDown.Position = [119 436 81 41];
            app.HypertensionDropDown.Value = '1';

            % Create FamilyHistoryofIADropDownLabel
            app.FamilyHistoryofIADropDownLabel = uilabel(app.UIFigure);
            app.FamilyHistoryofIADropDownLabel.HorizontalAlignment = 'right';
            app.FamilyHistoryofIADropDownLabel.FontWeight = 'bold';
            app.FamilyHistoryofIADropDownLabel.Position = [212 448 118 22];
            app.FamilyHistoryofIADropDownLabel.Text = 'Family History of IA';

            % Create FamilyHistoryofIADropDown
            app.FamilyHistoryofIADropDown = uidropdown(app.UIFigure);
            app.FamilyHistoryofIADropDown.Items = {'Yes', 'No', 'NR'};
            app.FamilyHistoryofIADropDown.ItemsData = {'1', '0', 'NR'};
            app.FamilyHistoryofIADropDown.ValueChangedFcn = createCallbackFcn(app, @FamilyHistoryofIADropDownValueChanged, true);
            app.FamilyHistoryofIADropDown.FontWeight = 'bold';
            app.FamilyHistoryofIADropDown.Position = [345 438 81 41];
            app.FamilyHistoryofIADropDown.Value = '1';

            % Create AgeEditFieldLabel
            app.AgeEditFieldLabel = uilabel(app.UIFigure);
            app.AgeEditFieldLabel.HorizontalAlignment = 'right';
            app.AgeEditFieldLabel.FontWeight = 'bold';
            app.AgeEditFieldLabel.Position = [468 559 27 22];
            app.AgeEditFieldLabel.Text = 'Age';

            % Create AgeEditField
            app.AgeEditField = uieditfield(app.UIFigure, 'text');
            app.AgeEditField.ValueChangedFcn = createCallbackFcn(app, @AgeEditFieldValueChanged, true);
            app.AgeEditField.Position = [510 558 103 24];

            % Show the figure after all components are created
            app.UIFigure.Visible = 'on';
        end
    end

    % App creation and deletion
    methods (Access = public)

        % Construct app
        function app = aneurysm_app_2023

            % Create UIFigure and components
            createComponents(app)

            % Register the app with App Designer
            registerApp(app, app.UIFigure)

            % Execute the startup function
            runStartupFcn(app, @startupFcn)

            if nargout == 0
                clear app
            end
        end

        % Code that executes before app deletion
        function delete(app)

            % Delete UIFigure when app is deleted
            delete(app.UIFigure)
        end
    end
end
