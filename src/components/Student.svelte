<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';

    export let studentData, index, width = 1300, height = 680;

    let anxietyData = [];

    onMount(() => {
        // Calculate proportions of students with and without anxiety
        const totalStudents = studentData.length;
        const studentsWithAnxiety = studentData.filter(student => student.anxiety === 'Yes').length;
        const studentsWithoutAnxiety = studentData.filter(student => student.anxiety === 'No').length;

        const margin = {top: 90, right: 50, bottom: 180, left: 120};
        const chartWidth = width - margin.left - margin.right;
        const chartHeight = height - margin.top - margin.bottom;

        // Create data array for pie chart
        anxietyData = [
            { label: 'With Anxiety', value: studentsWithAnxiety },
            { label: 'Without Anxiety', value: studentsWithoutAnxiety }
        ];

        // Remove any existing SVG elements
        d3.select('svg').selectAll('*').remove();

        // Create pie function
        const pie = d3.pie().value(d => d.value);

        // Create arc function
        const arc = d3.arc()
            .innerRadius(0)
            .outerRadius(Math.min(chartWidth, chartHeight) / 2 - 10);

        // Create SVG group for pie chart
        const svg = d3.select('svg')
            .attr('width', width)
            .attr('height', height)
            .append('g')
            .attr('transform', `translate(${margin.left + chartWidth / 6}, ${margin.top + chartHeight / 2})`); // Adjusted translation here

        // Draw pie slices
        const arcs = svg.selectAll('path')
            .data(pie(anxietyData))
            .enter()
            .append('g')
            .attr('class', 'arc');

        arcs.append('path')
            .attr('d', arc)
            .attr('fill', (d, i) => ['#e3c466', '#b0aea5'][i])
            .attr('stroke', 'white')
            .style('stroke-width', '2px')
            ;

        // Add text for proportion values
        arcs.append('text')
            .attr('transform', d => `translate(${arc.centroid(d)})`)
            .attr('text-anchor', 'middle')
            .attr('fill', 'white')
            .text(d => `${((d.endAngle - d.startAngle) / (2 * Math.PI) * 100).toFixed(2)}%`)
            .attr('font-family', 'Verdana')
            .attr('font-size', 14);

        svg.append('text')
            .attr('x', -112)
            .attr('y', -70)
            .text('Anxiety')
            .attr('font-family', 'Verdana')
            .attr('font-size', 14)
            .attr('fill', 'white');

        svg.append('text')
            .attr('x', 47)
            .attr('y', 25)
            .text('No Anxiety')
            .attr('font-family', 'Verdana')
            .attr('font-size', 14)
            .attr('fill', 'white');

        // Title
        svg.append('text')
            .attr('x', 0)
            .attr('y', 250)
            .attr('text-anchor', 'middle')
            .text('Anxiety Prevalence in College Students (%)')
            .attr('font-family', 'Verdana')
            .attr('font-size', 18)
            .attr('font-weight', 400);
    });
</script>

<main style="display: flex;">
    <svg style="flex: 1;"></svg>
    <div style="flex: 1; margin-right: 130px; margin-left: 50px; margin-top: 50px">
        <h1 style="font-family: Futura; font-size: 34px; margin-bottom: 20px; line-height: 3;">Anxiety Among College Students</h1>
        <div style="font-family: Verdana; font-size: 18px; text-align: justify; line-height: 1.6;">
            <p>College students are one of the most vulnerable populations when it comes to mental health concerns.</p>
            <p>In the Spring 2022 National College Health Assessment (American College Health Association, 2022), roughly one in three students reported being diagnosed with an anxiety disorder (e.g., generalized anxiety disorder, social anxiety disorder, panic disorder, and specific phobia), while about 35% of students reported anxiety as an impeding factor to academic performance.</p>
        </div>
    </div>
</main>






