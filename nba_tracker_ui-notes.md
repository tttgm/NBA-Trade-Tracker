# Creating NBA trade tracker UI

Started making react app for chord chart (using `nivo` charts)

## Created new react app
From in `datasciprojects` ran: `npx create-react-app nba-trades-ui`

Opened folder in VS Code.

Try to run app: `npm start`

Can leave this running while developing the app to see what's going on

## Import data (.json) files to project
Copy over the prepared .json files (the square matrix arrays for chord chart plotting) to start playing around with.

Recommend creating a `src/data/` folder to store the data files.

Changed the .json files to .js then assigned the arrays as `const matrix=` or whatever at the start, then at the bottom of the file added:
`export default matrix;`

Now it should be accesible from the JSX code.

## Trying out the nivo ChordChartCanvas
See: https://nivo.rocks/chord/canvas

DOCS ON NIVO.ROCKS ARE WRONG!! ResponsiveChordCanvas is in `@nivo/chord` NOT `@nivo/calendar`

## Added the slider for year selection
See: https://material-ui.com/lab/api/slider/

## Pushed to github then deployed on netlify
See: https://nba-trade-winds.netlify.com/



